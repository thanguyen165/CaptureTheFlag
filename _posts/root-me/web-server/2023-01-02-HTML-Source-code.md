---
title: HTML - Source code
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation]
---

* Points: 5
* Link: [https://www.root-me.org/en/Challenges/Web-Server/HTML-Source-code](https://www.root-me.org/en/Challenges/Web-Server/HTML-Source-code)

## Statement

> [http://challenge01.root-me.org/web-serveur/ch1/](http://challenge01.root-me.org/web-serveur/ch1/)

## Solution

View source code by browser, we will get nothing.

But by terminal, it is a bit different now:
```sh
curl http://challenge01.root-me.org/web-serveur/ch1/
```

```html
<html><body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>

<!--

Bienvenue sur ce portail,
Welcome on this portal,

J'espère que vous passerez un agréable moment parmi nous, mais surtout que vous repartirez plein de choses dans la tête...
I hope that you will enjoy your time among us, and above that all you will leave with lots of things in the head ...

@ très bientôt
See ya

-->
<h1>Login v0.00001</h1>

<form>
    Password&nbsp;<input type="password" value="" name="password"/><br/>
    <input type="submit" value="login" />
</form>





<!--
                                                           Je crois que c'est vraiment trop simple là !
                                                              It's really too easy !
                                                                   password : nZ^&@q5&sjJHev0


-->

</body></html>
```

The flag is the password above:
```
nZ^&@q5&sjJHev0
```
