---
title: Authentication
author: thanguyen165
date: 2023-07-12 07:00:00 +0700
categories: [Practice, w3challs]
tags: [Web Exploitation, w3challs]
---

* Points: 9

## Analyzation

Get into the ```administration``` page, it says
```
Forbidden Access!

This part of the site is reserved to the admin! 
```

Check cookie
```
b14361404c078ffd549c03db443c3fede2f3e534d73f78f77301ed97d4a436a9fd9db05ee8b325c0ad36438b43fec8510c204fc1c1edb21d0941c00e9e2c1ce2
```

This is the [SHA512](https://en.wikipedia.org/wiki/Secure_Hash_Algorithms) hash. Decrypt it, that value is hash of ```"user"```.

## Solution

SHA512 hash of ```"admin"``` is
```
c7ad44cbad762a5da0a452f9e854fdc1e0e7a52a38015f23f3eab1d80b931dd472634dfac71cd34ebc35d16ab7fb8a90c81f975113d6c7538dc69dd8de9077ec
```

Replace it to the cookie and get the flag.

The flag is
```
W3C{iaobjej4g}
```