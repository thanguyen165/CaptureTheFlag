---
title: Javascript - Authentication
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, rootme]
---

* Points: 5
* Link: [https://www.root-me.org/en/Challenges/Web-Client/Javascript-Authentication](https://www.root-me.org/en/Challenges/Web-Client/Javascript-Authentication)

## Analyzation

Firstly, check the site's HTML code and CSS code by browser, nothing special at all...

```html
<html>
<head>
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              <script type="text/javascript" src="login.js"></script>
</head>
<body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
    <fieldset style="margin-top: 10px; padding: 10px;" width="60%">
	<legend><b>Login</b></legend><br/>
	<form name="login" method="POST" action="">
	    Username : <input name="pseudo" /><br/>
	    Password : <input type="password" name="password" /></br></br>
	    <input onclick="Login()" type="button" value="login" name="button" />
	</form>
    </fieldset>
</body>
</html>
```

But be careful! The challenge's author try to fool us by entering a lot of space. There is a ```login.js``` file
```html
<script type="text/javascript" src="login.js"></script>
```

```javascript
/* <![CDATA[ */

function Login(){
	var pseudo=document.login.pseudo.value;
	var username=pseudo.toLowerCase();
	var password=document.login.password.value;
	password=password.toLowerCase();
	if (pseudo=="4dm1n" && password=="sh.org") {
	    alert("Password accepté, vous pouvez valider le challenge avec ce mot de passe.\nYou an validate the challenge using this password.");
	} else { 
	    alert("Mauvais mot de passe / wrong password"); 
	}
}
/* ]]> */
```

We get the username and password:
```
username=="4dm1n"  
password=="sh.org"
```

## Solution

Submit them to the form, the alert appeared. It told us:
```
You an validate the challenge using this password.
```

So, what are you waiting for?

The flag is
```
sh.org
```
