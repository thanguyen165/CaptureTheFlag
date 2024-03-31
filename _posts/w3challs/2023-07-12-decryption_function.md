---
title: Decryption function
author: thanguyen165
date: 2023-07-12 07:00:00 +0700
categories: [Practice, w3challs]
tags: [Cryptography]
---

* Points: 9

## Description

> You are provided with the following encryption function: ```f(x)=21x+11 [26]```
>
> First, you have to encrypt the word ```GOOGLE``` with this function.  
> Then, your mission is to find the decryption function ```g```.  
> Finally, you have to decrypt the word ```GELKT```.

### Details

- [26] means "modulo 26".  
- The decryption function will be of the following form: ```g(y)=ay+b[26]```, with ```a``` and ```b``` **natural integers lower than 26**, and ```y=f(x)```.
- You will enter the answer lowercase in the form
```
(crypted word)_(decryption function)_(decrypted word)
```

## Solution

```python
import string

alphabet = string.ascii_lowercase

def f(x: int) -> int:
    return (21 * x + 11) % 26

def f_inv(y: int) -> int:
    # y = 21 * x + 11
    # --> 21 * x = y - 11
    # --> 5 * 21 * x = 5 * (y - 11)
    # --> 1 * x = 5 * y - 3
    # --> x = 5 * y + 23
    return (5 * y + 23) % 26

def encrypt(str):
    return "".join(alphabet[f(alphabet.find(c))] for c in str)

def decrypt(str):
    return "".join(alphabet[f_inv(alphabet.find(c))] for c in str)

print(encrypt("google"))
print("----")
print(decrypt("gelkt"))
```

The flag is
```
W3C{httir_5y+23[26]_bravo}
```