---
title: Local File Inclusion
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, LFI]
---

* Points: 30
* Link: [https://www.root-me.org/en/Challenges/Web-Server/Local-File-Inclusion](https://www.root-me.org/en/Challenges/Web-Server/Local-File-Inclusion)

## Statement

> Get in the admin section.

## Analyzation

Click the challenge's link
![first click](/assets/posts_images/root-me/web-server/Local_File_Inclusion/origin.png){:max-width="100"}

Nothing much to do. Click on any tab.

![click tab](/assets/posts_images/root-me/web-server/Local_File_Inclusion/click-tab.png){:max-width="100"}

There is a query. So I guess ```files``` is for a directory.

Click on any file on the screen

![click file](/assets/posts_images/root-me/web-server/Local_File_Inclusion/click-file.png){:max-width="100"}

There is a new query. This time I guess ```f``` is for a file.

Let's do some path traversal

![travel back two times](/assets/posts_images/root-me/web-server/Local_File_Inclusion/travel-back-two-times.png){:max-width="100"}

So we can only travel back to one directory.

![travel back one time](/assets/posts_images/root-me/web-server/Local_File_Inclusion/travel-back-one-time.png){:max-width="100"}

There is it, ```admin```. But is it a file or a directory? Try both of them, I know it is a directory.

![admin directory](/assets/posts_images/root-me/web-server/Local_File_Inclusion/dir-admin.png){:max-width="100"}

```index.php``` file! Read it

![index.php](/assets/posts_images/root-me/web-server/Local_File_Inclusion/index.png){:max-width="100"}

Check these lines in that file

```php
$realm = 'PHP Restricted area';
$users = array('admin' => 'OpbNJ60xYpvAQU8');
```

That's our flag.

The flag is
```
OpbNJ60xYpvAQU8
```