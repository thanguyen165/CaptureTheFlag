---
title: Change your browser
author: thanguyen165
date: 2023-07-12 07:00:00 +0700
categories: [Practice, w3challs]
tags: [Web Exploitation, w3challs]
---

* Points: 1

## Analyzation

Get into the challenge's link, they said
```
To solve this challenge, your browser must be: W3Challs_browser

Your current browser is Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0
```

## Solution

Do what they ask

```sh
curl -H "User-agent: W3Challs_browser" https://change-browser.hax.w3challs.com
```

The flag is
```
W3C{now_that_we_have_the_right_browser_lets_get_the_party_started}
```