---
title: 2023 ImaginaryCTF - chaos
author: allforest01
date: 2023-07-23 00:00:00 +0700
categories: [Write-ups, 2023_imaginaryCTF]
tags: [RE]
---

* Points: 100

## Description

> Can you find order in the midst of chaos?

### Attached

[chaos.py](https://imaginaryctf.org/f/orm4L#chall.py)

## Solution

By [allforest01](https://github.com/allforest01)

```python
from math import exp, log

code = open("chall.py").read()
inp_str = "\\x36\\x39\\x20\\x36\\x65\\x20\\x37\\x30"
ans = [0 for i in range(51)]
pos = 0
while True:
    pos = code.find(inp_str, pos)
    if pos == -1:
        break
    pos = code.find("__getitem__(", pos) + len("__getitem__(")
    nxt = code.find(")", pos)
    ans_pos = eval(code[pos:nxt])
    pos = code.find("__pow__(", pos) + len("__pow__(")
    nxt = code.find(")", pos)
    ans_pow = int(code[pos:nxt])
    pos = code.find("__eq__(", pos) + len("__eq__(")
    nxt = code.find(")", pos)
    ans_equ = int(code[pos:nxt])
    ans[ans_pos] = chr(round(exp(log(ans_equ) / ans_pow)))

for i in ans:
    print(i, end='')
print()
```

The flag is
```
ictf{pYthOn_obFuScAtION_iS_N0_M4TCH_f0r_U_9e1b23f9}
```