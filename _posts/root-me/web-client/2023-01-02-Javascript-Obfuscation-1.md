---
title: Javascript - Obfuscation 1
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, rootme]
---

* Points: 10
* Link: [https://www.root-me.org/en/Challenges/Web-Client/Javascript-Obfuscation-1](https://www.root-me.org/en/Challenges/Web-Client/Javascript-Obfuscation-1)

## Solution

Let's visit the site's source code:

```html
<html>
    <head>
        <title>Obfuscation JS</title>

          <script type="text/javascript">
              /* <![CDATA[ */

              pass = '%63%70%61%73%62%69%65%6e%64%75%72%70%61%73%73%77%6f%72%64';
              h = window.prompt('Entrez le mot de passe / Enter password');
              if(h == unescape(pass)) {
                  alert('Password accepté, vous pouvez valider le challenge avec ce mot de passe.\nYou an validate the challenge using this pass.');
              } else {
                  alert('Mauvais mot de passe / wrong password');
              }

              /* ]]> */
          </script>
    </head>
   <body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
    </body>
</html>
```

Run
```javascript
unescape(%63%70%61%73%62%69%65%6e%64%75%72%70%61%73%73%77%6f%72%64)
```

We will have a string:
```
cpasbiendurpassword
```

They said:
```
You an validate the challenge using this pass.
```

The flag is:
```
cpasbiendurpassword
```
