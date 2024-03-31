---
title: Javascript - Obfuscation 2
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation]
---

* Points: 10
* Link: [https://www.root-me.org/en/Challenges/Web-Client/Javascript-Obfuscation-2](https://www.root-me.org/en/Challenges/Web-Client/Javascript-Obfuscation-2)

## Description

> Going down 3 floors.....

## Solution

This is the site's HTML code:
 ```html
<html>

<head>
        <title>Obfuscation JS</title>
<!--
Obfuscation
.Niveau : Facile
.By Hel0ck
.The mission :
        Retrouver le password contenu dans la var pass.
        You need my help ? IRC : irc.root-me.org #root-me
-->
<script type="text/javascript">
        var pass = unescape("unescape%28%22String.fromCharCode%2528104%252C68%252C117%252C102%252C106%252C100%252C107%252C105%252C49%252C53%252C54%2529%22%29");
</script>
</head>

</html>
```

Check this line:
```javascript
unescape("unescape%28%22String.fromCharCode%2528104%252C68%252C117%252C102%252C106%252C100%252C107%252C105%252C49%252C53%252C54%2529%22%29");
```

Open console in browser, run it and we got:
```javascript
unescape("String.fromCharCode%28104%2C68%2C117%2C102%2C106%2C100%2C107%2C105%2C49%2C53%2C54%29")
```

Run this one, we got this:
```javascript
String.fromCharCode(104,68,117,102,106,100,107,105,49,53,54)
```

Continue, we got:
```javascript
hDufjdki156
```

Submit that pass for validation.
