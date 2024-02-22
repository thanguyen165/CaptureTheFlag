---
title: Backup file
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, rootme]
---

* Points: 15
* Link: [https://www.root-me.org/en/Challenges/Web-Server/Backup-file](https://www.root-me.org/en/Challenges/Web-Server/Backup-file)

## Statement

> No clue.

## Solution

```sh
dirsearch -u http://challenge01.root-me.org/web-serveur/ch11/
```

```sh
200 -  531B  - /web-serveur/ch11/index.php
200 -  843B  - /web-serveur/ch11/index.php~
```

Check both of them, we have
```php
<?php

$username="ch11";
$password="OCCY9AcNm1tj";


echo '
      <html>
      <body>
	<h1>Authentication v 0.00</h1>
';

if ($_POST["username"]!="" && $_POST["password"]!=""){
    if ($_POST["username"]==$user && $_POST["password"]==$password)
    {
      print("<h2>Welcome back {$row['username']} !</h2>");
      print("<h3>Your informations :</h3><p>- username : $row[username]</p><br />");
      print("To validate the challenge use this password</b>");
    } else {
      print("<h3>Error : no such user/password</h2><br />");

    }
}

echo '
	<form action="" method="post">
	  Login&nbsp;<br/>
	  <input type="text" name="username" /><br/><br/>
	  Password&nbsp;<br/>
	  <input type="password" name="password" /><br/><br/>
	  <br/><br/>
	  <input type="submit" value="connect" /><br/><br/>
	</form>
      </body>
      </html>
';

?> 
```

The flag is
```
OCCY9AcNm1tj
```