---
title: 2023 ImaginaryCTF - idoriot
author: thanguyen165
date: 2023-07-23 00:00:00 +0700
categories: [Write-ups, 2023_imaginaryCTF]
tags: [Web Exploitation, Authentication vulnerabilities]
---

* Points: 100

## Description

> Some idiot made this web site that you can log in to. The idiot even made it in php. I dunno.

## Analization

When registering, this code shows up

```php
Welcome, User ID: 231943960
Source Code
<?php

session_start();

// Check if user is logged in
if (!isset($_SESSION['user_id'])) {
    header("Location: login.php");
    exit();
}

// Check if session is expired
if (time() > $_SESSION['expires']) {
    header("Location: logout.php");
    exit();
}

// Display user ID on landing page
echo "Welcome, User ID: " . urlencode($_SESSION['user_id']);

// Get the user for admin
$db = new PDO('sqlite:memory:');
$admin = $db->query('SELECT * FROM users WHERE user_id = 0 LIMIT 1')->fetch();

// Check if the user is admin
if ($admin['user_id'] === $_SESSION['user_id']) {
    // Read the flag from flag.txt
    $flag = file_get_contents('flag.txt');
    echo "<h1>Flag</h1>";
    echo "<p>$flag</p>";
} else {
    // Display the source code for this file
    echo "<h1>Source Code</h1>";
    highlight_file(__FILE__);
}

?> 
```

These are important lines

```php
$admin = $db->query('SELECT * FROM users WHERE user_id = 0 LIMIT 1')->fetch();
```

```php
if ($admin['user_id'] === $_SESSION['user_id'])
```

So user_id of admin is 0. We need seesion's user_id is 0, too.

Check the register site's source code

```html

<!DOCTYPE html>
<html>
<head>
    <title>Registration</title>
</head>
<body>
    <h1>Registration</h1>
    
    <p>The user database will be wiped every 30 minutes.</p>
    <form method="POST" action="register.php">
        <label for="username">Username</label>
        <input type="text" name="username" id="username" required>
        <br>
        <label for="password">Password</label>
        <input type="password" name="password" id="password" required>
        <br>
        <input type="hidden" name="user_id" value="122296903">
        <input type="submit" value="Register">
    </form>

    <p>Already have an account? <a href="login.php">Login</a></p>
</body>
</html>
```

Here it is
```html
<input type="hidden" name="user_id" value="122296903">
```

It can be changed by inspect mode

## Solution

Edit the user_id before registering, you will have the flag

The flag is
```
ictf{1ns3cure_direct_object_reference_from_hidden_post_param_i_guess}
```
