---
title: Install files
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, rootme]
---

* Points: 15
* Link: [https://www.root-me.org/en/Challenges/Web-Server/Install-files](https://www.root-me.org/en/Challenges/Web-Server/Install-files)

## Statement

> You know phpBB ?

## Solution

```sh
curl -i http://challenge01.root-me.org/web-serveur/ch6/
```
```html
<!--  /web-serveur/ch6/phpbb -->
```

Get into that path
```sh
curl -i http://challenge01.root-me.org/web-serveur/ch6/phpbb
```

Nothing here! Let's try [dirsearch](https://www.kali.org/tools/dirsearch/)
```sh
dirsearch -u  http://challenge01.root-me.org/web-serveur/ch6/
```

Still nothing! Wait the minute, why the ```/phpbb``` does not appear...? Let's check it
```sh
dirsearch -u  http://challenge01.root-me.org/web-serveur/ch6/phpbb/
```
```sh
200 -   12KB - /web-serveur/ch6/phpbb/install/
```

Get into it and you will have what you want
```sh
curl http://challenge01.root-me.org/web-serveur/ch6/phpbb/install/install.php
```

The flag is
```
karambar
```