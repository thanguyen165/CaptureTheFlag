---
title: 2023 ImaginaryCTF - web
author: thanguyen165
date: 2023-07-23 00:00:00 +0700
categories: [Write-ups, 2023_imaginaryCTF]
tags: [Forensics]
---

* Points: 100

## Description

> We recovered this file from the disk of a potential threat actor. Can you find out what they were up to?

### Attached

[web.zip](https://imaginaryctf.org/r/y1V79#web.zip)

## Analyzation

Check the ```login.json``` file

```json
{"nextId":2,
"logins":[
    {"id":1,
    "hostname":"https://yoteachapp.com",
    "httpRealm":null,
    "formSubmitURL":"https://yoteachapp.com",
    "usernameField":"",
    "passwordField":"",
    "encryptedUsername":"MDIEEPgAAAAAAAAAAAAAAAAAAAEwFAYIKoZIhvcNAwcECJs6PTFwzrMiBAiRmXcD4tn3bw==",
    "encryptedPassword":"MGIEEPgAAAAAAAAAAAAAAAAAAAEwFAYIKoZIhvcNAwcECBZPCW+NjkpUBDieso9w5lPvD85RNcErLbGTXdamyji7ZKcL9FHxjnvt1WqwcVCsOETgCWCgwCg1jJmAW/MYugOoqQ==",
    "guid":"{8ee7f027-974b-48cb-b9aa-29fc5a728c39}",
    "encType":1,
    "timeCreated":1688943236140,
    "timeLastUsed":1688943236140,
    "timePasswordChanged":1688943236140,
    "timesUsed":1,
    "encryptedUnknownFields":null}],
"potentiallyVulnerablePasswords":[],
"dismissedBreachAlertsByLoginGUID":{},
"version":3}
```

We need to login to [yoteachapp.com](https://yoteachapp.com) by the given encoded account.

## Solution

Use [firefox_decrypt.py](https://github.com/unode/firefox_decrypt) to decrypt the account

```sh
py firefox_decrypt/firefox_decrypt.py ./.mozilla/firefox/
```

```
Website:  https://yoteachapp.com
Username: ''
Password: 'UeMBYIbgPqNiSWzOVguTbccMOnLirDoEGTjgiqNrbOvwzynbyN'
```

Login and find the flag.

The flag is

```
ictf{behold_th3_forensics_g4untlet_827b3f13}
```