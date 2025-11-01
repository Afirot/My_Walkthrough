# Summary

![summary](img/image.webp)


# Port enumeration

I used nmap to enumerate all the open ports.

```bash
nmap -p- -T5 -Pn -vvv -n 10.10.11.105 -oG allPorts.txt
```

![nmap results](img/image11.png)

I just found 2 ports oppen, 22 and 80, ssh and http.

![web](img/image29.png)

This web page is simple, I didnt found nothing there, so, I started to enumerate possible directories and subdomains.

# Subdomains enumeration

Using wfuzz, I found something interesting.

![wfuzz](img/image28.png)

By accessing the browser's subdomain, I only found this.

![welcome](img/image18.png)

Very funny, but, after enumerate all the directorys in this subdomains, I found some interesting routes.

![wfuzz2](img/image1.png)

In reviews, I found a json with some users name.

![reviews](img/image15.png)

In admin I found a strapi instance.

![strapi](img/image4.png)

# Strapi enumeration

After reading the files, I found the strapi instance version.

![strapiVer](img/image30.png)

After a fast google search, I found who this version is vulnerable to CVE-2019-18818, wich allows me to reset users password abusing a vulnerable reset-password page.

# Exploiting CVE-2019-18818

I just need to send an change-password request.

![strapiVer](img/image14.png)

After this, the app will acept our request and we will send another request who will contain the new admin password.

![strapiVer](img/image27.png)

Now, I have access to the admin account.

![admin](img/image12.png)

# Admin panel enumeration

After a google search, I found another exploitable vulnerability who allow me to execute Arbitrary Code Injection.

![vuln2](img/image24.png)

After a wile, I found an exploit.

![exploit](img/image26.png)

This exploit will try to install an plugin and, with the documentation, will inject the code.

# Reverse shell

With this script, I obtained an Reverse Shell

![exploit](img/image9.png)

# Local enumeration

With /etc/passwd i found the 2 users, developer and strapi, I am strapi at this point.

With netstat, i found something strange working in port 8000.

![netstat](img/image8.png)

# Port fowarding

I will use chisel to redirect this service.

First, I will send the binary to the victim system.

![getchisel](img/image23.png)

Next, I started the server on the attacker machine.

![serverchisel](img/image25.png)

On the victim, I started the tunnel client.

![clientchisel](img/image2.png)

With that tunnel, I could access the hidden service, which turned out to be a Laravel instance.

![laravel](img/image13.png)

# Laravel enumeration

A quick search revealed CVE-2021-3129 and an exploit that allows remote code execution.

![laravelcve](img/image3.png)

![laravelexploit](img/image21.png)

This exploit abuses two insecure functions in the PHP code, which allow us to execute commands.

After running the exploit, I found that I had become root.

![root](img/image16.png)