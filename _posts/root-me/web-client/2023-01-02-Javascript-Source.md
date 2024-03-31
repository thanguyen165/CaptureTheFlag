---
title: Javascript - Source
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation]
---

* Points: 5
* Link: [https://www.root-me.org/en/Challenges/Web-Client/Javascript-Source](https://www.root-me.org/en/Challenges/Web-Client/Javascript-Source)

## Description

> You know javascript ?

## Solution

Let's visit the site's source code:
```sh
curl http://challenge01.root-me.org/web-client/ch1/
```

```html
<html>
    <head>
        <script type="text/javascript">
        /* <![CDATA[ */
            function login(){
                pass=prompt("Entrez le mot de passe / Enter password");
                if ( pass == "123456azerty" ) {
                    alert("Mot de passe accepté, vous pouvez valider le challenge avec ce mot de passe.\nYou can validate the challenge using this password.");  }
                else {
                    alert("Mauvais mot de passe / wrong password !");
                }
            }
        /* ]]> */
        </script>
    </head>
   <body onload="login();"><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>

    </body>
</html>
```

As you can see in the code, we have:
```javascript
pass == "123456azerty"
```

They also said:
```
You can validate the challenge using this password.
```

The flag is:
```
123456azerty
```
