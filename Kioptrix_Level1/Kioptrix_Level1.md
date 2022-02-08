---
title: Kioptrix Level 1 - Vulnhub VM Challenge
author: 47Suriya
author_url: https://twitter.com/_47Suriya_
categories: 
 - Vulnhub
tags:
 - Vulnhub
 - Write up
 - Kioptrix Level 1
 - Kioptrix 
 - VM Challenge
---

---
# Kioptrix Level 1 - Vulnhub VM Challenge

## Description
This Kioptrix VM Image are easy challenges. The object of the game is to acquire root access via any means possible (except actually hacking the VM server or player). The purpose of these games are to learn the basic tools and techniques in vulnerability assessment and exploitation. There are more ways then one to successfully complete the challenges.
<br>


![0.png](Kioptrix_Level1\0.png)

## Finding the IP of the Target Machine

The IP of the target machine is found by using [Netdiscover tool](https://github.com/alexxy/netdiscover).

Here the IP of the vulnerable machine is **192.168.1.230**
## Initial Scan 

First, [Nmap scanner](https://github.com/nmap/nmap) is used to find all the open ports and services running. 
And here we find that Apache 1.3.20 is running
on port 80

It's time to do some more recon. 

We use [Nikto Web scanner](https://github.com/sullo/nikto) to find all those server side vulnerabilities and it shows some details about the Apache 1.3.2 vulnerabilities

## Exploit

When we search about Apache/1.3.20 and mod_ssl/2.8.4 we can see that there is an exploit for ‘OpenLuckV2.c' Remote Buffer Overflow. So we download the [Exploit](https://github.com/heltonWernik/OpenLuck).
<br>
And then we run it.
After running the exploit we will get root access in the Target machine <3