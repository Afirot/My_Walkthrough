# Summary

![cover](img/cover.png)

Validation is a easy CTF from the plataform Hack The Box, where we will neet to obtain a shell with a SQLI.

# Port enumeration

With nmap I found 4 ports oppen, 22, 80, 4566 and 8080

![nmap](img/image17.png)

![nmap](img/image10.png)

The HTTP server in port 80 was the only one with a real page working.

# Web enumeration

The only thing I can see is a form who allow me to register a username and the country where I am, the name imput wasn't vulnerable to SQLI.

![form](img/image9.png)

But that cannot be said about the country imput, who is completly vulnerable, we can change it with burpsuite or caido.

![burp](img/image11.png)

![sqli1](img/image19.png)

# SQLI Exploitation

Using this vector, I started to take information.

![sqli2](img/image18.png)

The sql user.

![sqli2](img/image21.png)

The database name.

![sqli3](img/image7.png)

The /etc/passwd file.

## Shell

After all this, I tried to create a php shell file using SQL.

![sqli4](img/image16.png)

It worked

![sqli5](img/image12.png)

Using the same tecnique, I created a reverse shell script.

![sqli5](img/image13.png)

Then, I executed it using shell.php and I have my shell.

![shell](img/image4.png)

# Local enumeration

Soon I see in the file config.php the uhc user password

![priv1](img/priv(1).png)

# Privilege scalation

Using this pasword, I was able to log as root.

![priv1](img/priv(2).png)