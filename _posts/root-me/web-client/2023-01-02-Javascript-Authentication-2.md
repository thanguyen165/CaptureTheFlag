---
title: Javascript - Authentication 2
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, rootme]
---

* Points: 10
* Link: [https://www.root-me.org/en/Challenges/Web-Client/Javascript-Authentication-2](https://www.root-me.org/en/Challenges/Web-Client/Javascript-Authentication-2)

## Description

> Yes folks, Javascript is damn easy :)

## Solution

Let's visit the site's source code by this command

```sh
curl http://challenge01.root-me.org/web-client/ch11/
```

```html
<html>
    <head>
        <title>JS Authentication</title>
        <script language="JavaScript" src="login.js"></script>
    </head>
   <body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
        <div id=EchoTopic>
        <p>Authentication</p>
        <p><input type="button" value="login" onclick="connexion();"></p>
        <br/><br/>
        <a href="javascript:window.close();">Close Window</a>
        </div>
    </body>
</html>
```

Let's check the ```login.js``` file:

```sh
curl http://challenge01.root-me.org/web-client/ch11/login.js
```

```javascript
function connexion(){
    var username = prompt("Username :", "");
    var password = prompt("Password :", "");
    var TheLists = ["GOD:HIDDEN"];
    for (i = 0; i < TheLists.length; i++)
    {
        if (TheLists[i].indexOf(username) == 0)
        {
            var TheSplit = TheLists[i].split(":");
            var TheUsername = TheSplit[0];
            var ThePassword = TheSplit[1];
            if (username == TheUsername && password == ThePassword)
            {
                alert("Vous pouvez utiliser ce mot de passe pour valider ce challenge (en majuscules) / You can use this password to validate this challenge (uppercase)");
            }
        }
        else
        {
            alert("Nope, you're a naughty hacker.")
        }
    }
}
```

We have:
```
username=="GOD"  
password=="HIDDEN"
```
See [JavaScript String split()](https://www.w3schools.com/jsref/jsref_split.asp) for more information.

They said:
```
You can use this password to validate this challenge (uppercase)
```

The flag is:
```
HIDDEN
```
