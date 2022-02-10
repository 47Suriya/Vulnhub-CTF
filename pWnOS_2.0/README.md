
---
# pWnOS: 2.0 - Vulnhub VM Challenge

## Description
**Goal:** <br>
Get root... Win! <br>

**About**: <br>
pWnOS v2.0 is a Virutal Machine Image which hosts a server to pratice penetration testing. It will test your ability to exploit the server and contains multiple entry points to reach the goal (root). It was design to be used with WMWare Workstation 7.0, but can also be used with most other virtual machine software.

**Configuration & Setup:** <br>
Configure your attacking platform to be within the **10.10.10.0/24** network range. <br>

The ip of the attacking machine can be set within the specified range using the commandsd below:

```bash
sudo ifconfig eth0 <your-ip> down
```
```bash
sudo ifconfig eth0 10.10.10.101 up
```
**Server's Network Settings:** <br>

**IP:** 10.10.10.100
<br>
**Netmask:** 255.255.255.0
<br>
**Gateway:** 10.10.10.15
<br>

## Finding the IP of the Target Machine

Here the static ip of the lab is already provided as 10.10.10.100 so we can proceed to run
some scans on this ip.

## Initial Scan 

First, [Nmap scanner](https://github.com/nmap/nmap) is used to find all the open ports and services running. 

![nmap](images/0.png)

From this scan, we can see that port 22 and 80 are open. **SSH** service is running on port 22
and an **Apache** server is open on port 80. 
<br>

When we go to the webpage hosted on port 80

![webpage](images/1.png)

Now [Dirb](https://github.com/v0re/dirb) web content scanner is used to find some information about some hidden directories

```bash
┌──(kali㉿kali)-[~]
└─$ dirb http://10.10.10.100/           

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Sat Jun 26 16:11:23 2021
URL_BASE: http://10.10.10.100/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.10.100/ ----
+ http://10.10.10.100/activate (CODE:302|SIZE:0)                                                                                              
==> DIRECTORY: http://10.10.10.100/blog/                                                                                                      
+ http://10.10.10.100/cgi-bin/ (CODE:403|SIZE:288)                                                                                            
==> DIRECTORY: http://10.10.10.100/includes/                                                                                                  
+ http://10.10.10.100/index (CODE:200|SIZE:854)                                                                                               
+ http://10.10.10.100/index.php (CODE:200|SIZE:854)                                                                                           
+ http://10.10.10.100/info (CODE:200|SIZE:50175)                                                                                              
+ http://10.10.10.100/info.php (CODE:200|SIZE:50044)                                                                                          
+ http://10.10.10.100/login (CODE:200|SIZE:1174)                                                                                              
+ http://10.10.10.100/register (CODE:200|SIZE:1562)                                                                                           
+ http://10.10.10.100/server-status (CODE:403|SIZE:293)                                                                                       
                                                                                                                                              
---- Entering directory: http://10.10.10.100/blog/ ----
+ http://10.10.10.100/blog/add (CODE:302|SIZE:0)                                                                                              
+ http://10.10.10.100/blog/atom (CODE:200|SIZE:1062)                                                                                          
+ http://10.10.10.100/blog/categories (CODE:302|SIZE:0)                                                                                       
+ http://10.10.10.100/blog/comments (CODE:302|SIZE:0)                                                                                         
==> DIRECTORY: http://10.10.10.100/blog/config/                                                                                               
+ http://10.10.10.100/blog/contact (CODE:200|SIZE:5898)                                                                                       
==> DIRECTORY: http://10.10.10.100/blog/content/                                                                                              
+ http://10.10.10.100/blog/delete (CODE:302|SIZE:0)                                                                                           
==> DIRECTORY: http://10.10.10.100/blog/docs/                                                                                                 
==> DIRECTORY: http://10.10.10.100/blog/flash/                                                                                                
==> DIRECTORY: http://10.10.10.100/blog/images/                                                                                               
+ http://10.10.10.100/blog/index (CODE:200|SIZE:8094)                                                                                         
+ http://10.10.10.100/blog/index.php (CODE:200|SIZE:8094)                                                                                     
+ http://10.10.10.100/blog/info (CODE:302|SIZE:0)                                                                                             
+ http://10.10.10.100/blog/info.php (CODE:302|SIZE:0)                                                                                         
==> DIRECTORY: http://10.10.10.100/blog/interface/                                                                                            
==> DIRECTORY: http://10.10.10.100/blog/languages/                                                                                            
+ http://10.10.10.100/blog/login (CODE:200|SIZE:5647)                                                                                         
+ http://10.10.10.100/blog/logout (CODE:302|SIZE:0)                                                                                           
+ http://10.10.10.100/blog/options (CODE:302|SIZE:0)                                                                                          
+ http://10.10.10.100/blog/rdf (CODE:200|SIZE:1411)                                                                                           
+ http://10.10.10.100/blog/rss (CODE:200|SIZE:1237)                                                                                           
==> DIRECTORY: http://10.10.10.100/blog/scripts/                                                                                              
+ http://10.10.10.100/blog/search (CODE:200|SIZE:4931)                                                                                        
+ http://10.10.10.100/blog/setup (CODE:302|SIZE:0)                                                                                            
+ http://10.10.10.100/blog/static (CODE:302|SIZE:0)                                                                                           
+ http://10.10.10.100/blog/stats (CODE:200|SIZE:5289)                                                                                         
==> DIRECTORY: http://10.10.10.100/blog/themes/                                                                                               
+ http://10.10.10.100/blog/trackback (CODE:302|SIZE:0)                                                                                        
+ http://10.10.10.100/blog/upgrade (CODE:302|SIZE:0)                                                                                          
                                                                                                                                              
---- Entering directory: http://10.10.10.100/includes/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                              
---- Entering directory: http://10.10.10.100/blog/config/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                              
---- Entering directory: http://10.10.10.100/blog/content/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                              
---- Entering directory: http://10.10.10.100/blog/docs/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                              
---- Entering directory: http://10.10.10.100/blog/flash/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                              
---- Entering directory: http://10.10.10.100/blog/images/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                              
---- Entering directory: http://10.10.10.100/blog/interface/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                              
---- Entering directory: http://10.10.10.100/blog/languages/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                              
---- Entering directory: http://10.10.10.100/blog/scripts/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                                                                                              
---- Entering directory: http://10.10.10.100/blog/themes/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                               
-----------------
END_TIME: Sat Jun 26 16:11:35 2021
DOWNLOADED: 9224 - FOUND: 30
```

Here, we can find that there is a directory named **blog** and also some other directories.

When we navigate to the directory,

![blog](images/4.png)

And when we check the page source

![source](images/5.png)

It shows that this website runs on [**Simple PHP Blog 0.4.0**](https://www.exploit-db.com/exploits/1191) 
<br>

When we search about that, we will be able to find about a **Perl** script

![exploitdb](images/6.png)

They have also mentioned the instructions for using the script

```bash
	Usage	: $0 [-h host] [-e exploit]
	
		-?      : this menu
		-h      : host
		-e	: exploit
			(1)	: Upload cmd.php in [site]/images/
			(2)	: Retreive Password file (hash)
			(3)	: Set New User Name and Password
				[NOTE - uppercase switches for exploits]
				-U	: user name
				-P	: password
			(4)	: Delete a System File
				-F	: Path and System File 

	Examples: $0 -h 127.0.0.1 -e 2
		  $0 -h 127.0.0.1 -e 3 -U l33t -P l33t
		  $0 -h 127.0.0.1 -e 4 -F ./index.php
		  $0 -h 127.0.0.1 -e 4 -F ../../../etc/passwd
		  $0 -h 127.0.0.1 -e 1
```

So we download this script and execute it using the command below to add a username and password of our choice

```bash
perl 1191.pl -h http://10.10.10.100/blog -e 3 -U agent47 -P agent47
```

![exploit](images/7.png)

Now after adding the username and password, we can login to the blog with those credentials.

And when we login,

![login-success](images/8.png)

We can find that there is an update in the menu section. Here the **Upload Image** option is a bit
sketchy. So when we try clicking that,

![upload-img](images/9.png)

We were actually able to upload files. So here we can try to upload a reverse shell to proceed
further. Here, a [PHP reverse shell](http://pentestmonkey.net/tools/web-shells/php-reverse-shell) from Pentest Monkey is uploaded.

**Note:** Before uploading the reverse.php file, make sure to change the ip to the ip of the
attacking machine and port that you want to spawn the shell

![upload-shell](images/10.png)

After uploading, we check the directory **blog/images** which we found in the initial **dirb** scan. 
When we navigate to the directory, we can see that the file **reverse.php** is actually uploaded and is ready to run.

![dir](images/11.png)

Now we can set up a **netcat** listener on our machine and open the **reverse.php** file to spawn a reverse shell
like the one below:

```bash
nc -lvnp 1234
```

![rev-shell](images/12.png)

When we list the directory we can find a var folder and then when we list the contents in the
var directory,

![var-dir](images/13.png)

There was something odd in this directory. A **mysqli_connect.php** file was present in the var directory itself. It might be indicating something. 
So when we display the file **mysqli_connect.php**, we get a username and a password

![mysqli-connect](images/14.png)

As there was an **SSH** service running in this machine, we try to login with these root credentials.

![ssh-login](images/15.png)

Finally, root access is acquired <3


