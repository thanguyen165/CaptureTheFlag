---
title: HTML - disabled buttons
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, rootme]
---

* Points: 5
* Link: [https://www.root-me.org/en/Challenges/Web-Client/HTML-disabled-buttons](https://www.root-me.org/en/Challenges/Web-Client/HTML-disabled-buttons)

## Description

> HTML protection?

### Statement

> This form is disabled and can not be used. It’s up to you to find a way to use it.

## Analyzation

This is HTML code of the site:
```html
<html>
    <head>
        <title>Under construction</title>
        <link rel='stylesheet' property='stylesheet' type='text/css' href='style.css' media='all' />
    </head>
   <body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
        <h1>Website temporarily closed.</h1>
        <hr>

        <form action="" method="post" name="authform">
            <div>
                <input disabled type="text" name="auth-login" value="" />
                <input disabled type="submit" value="Member access" name="authbutton" />
            </div>
        </form>
            </body>
</html>
```

Let's check these lines:
```html
<input disabled type="text" name="auth-login" value="" />
<input disabled type="submit" value="Member access" name="authbutton" />
```

## Solution

Open the inspect mode on the browser, delete ```disable```, and the submit button is able again.  
Submit anything to the web, and you will have the flag.

The flag is:
```
HTMLCantStopYou
```
