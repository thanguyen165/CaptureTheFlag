---
title: HTTP - Directory indexing
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, Path traversal]
---

* Points: 15
* Link: [https://www.root-me.org/en/Challenges/Web-Server/HTTP-Directory-indexing](https://www.root-me.org/en/Challenges/Web-Server/HTTP-Directory-indexing)

## Description

> CTRL + U...

## Solution

Inspect the code, there is an important line
```html
<!-- include("admin/pass.html") -->
```

Navigate to it, we got nothing!

But travel back to ```admin/```, there is a ```backup/``` directory. Found the ```admin.txt```. Password is in that file.

The flag is
```
LINUX
```