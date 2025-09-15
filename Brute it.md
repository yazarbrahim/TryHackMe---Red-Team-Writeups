### **https://tryhackme.com/room/bruteit**

### **Task 2: Reconnaissance**

**Q1: Firstly, we need to run a nmap scan to find out which ports are open and which services are running on these ports.**

**Nmap (Network Mapper)** is a free and open source utility for network discovery and security auditing.

nmap -A -T4 -O -p- -sV

```
nmap -A -T4 -O -p- 10.10.145.133
```

**\-A :** Enable OS detection, version detection, script scanning and traceroute.

**\-T4 :** Set timing template (higher is faster).

**\-O :** Enable OS detection.

**\-p- :** You can specify **“-p-”** to scan ports from 1 through 65535. ( We have to search all ports because question is asking total of open ports )

![2c42909e1c50489ab51c4d15bc8c73e5.png](resources/2c42909e1c50489ab51c4d15bc8c73e5.png)

**Q2: What version of SSH is running?**

**A2:** OpenSSH 7.6p1dir

**The SSH** protocol (also referred to as **Secure Shell**) is a method for secure remote login from one computer to another.

**Q3: What version of Apache is running?**

2.4.29

**<span style="color: #151c2b;">Q4: Which Linux distribution is running?</span>**

<span style="color: #151c2b;">Ubuntu</span>

<span style="color: #151c2b;">![a4349e6fac6e4d388cce9e9d1e6c20cd.png](resources/a4349e6fac6e4d388cce9e9d1e6c20cd.png)</span>

**<span style="color: #151c2b;">Q5: <span style="color: #151c2b;">Search for hidden directories on web server.</span> <span style="color: #151c2b;">What is the hidden directory?</span></span>**

<span style="color: #151c2b;"><span style="color: #151c2b;">/admin</span></span>

<span style="color: #151c2b;"><span style="color: #151c2b;">![91ba7954460d1ae36fd91a4d9fb9e56f.png](resources/91ba7954460d1ae36fd91a4d9fb9e56f.png)</span></span>

&nbsp;

![f3c6ad1ed04e6eedfb772f1e7255b705.png](resources/f3c6ad1ed04e6eedfb772f1e7255b705.png)

&nbsp;

&nbsp;

### **\==============================================================================================================**

### **Task 3: Getting Shell**

**Q1: <span style="color: #151c2b;">What is the user:password of the admin panel?</span>**

<span style="color: #151c2b;">admin:xavier</span>

<span style="color: #151c2b;">\--> We know the username in page source</span>

<span style="color: #151c2b;">![b36d544909d9612441ec6109bed27878.png](resources/b36d544909d9612441ec6109bed27878.png)</span>

<span style="color: #151c2b;">\--> We need to hydra to find password</span>

hydra -l admin -P /root/Desktop/wordlists/rockyou.txt .....ip...  http-post-form '/admin/:user=^USER^&pass=^PASS^:Username or password invalid'

When we try the default username and password, we get the following error message. You’re right, it can’t be that easy.

<img alt="" class="bg lm nr c jop-noMdConv" width="500" height="354" loading="eager" role="presentation" src="resources/1_us_-CXpuDNrfW6xSgRU1ww.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 500px; max-width: 100%; height: auto;">

**/admin**

By looking at the request, we can see that the website is using the **POST** method to login.

<img alt="" class="bg lm nr c jop-noMdConv" width="700" height="284" loading="lazy" role="presentation" src="resources/1_-QnksapAJ-AROW_5ADgRiQ.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 680px; max-width: 100%; height: auto;">

**Inspect / Network**

**POST** is the HTTP method that is designed to send loads of data to a server from a specified resource. Most common HTML forms on the web operate using this request method.

We see a section called **Request payload** that contains the username and password we entered. We need all of this request for Hydra to use.

<img alt="" class="bg lm nr c jop-noMdConv" width="547" height="307" loading="lazy" role="presentation" src="resources/1_BVAY7aojXT3o99CbDJ3YSQ.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 547px; max-width: 100%; height: auto;">

**Request**

Finally, we just need a way to let Hydra know whether or not we successfully logged-in. Since we can’t see what the page looks like upon a successful login, we’ll need to specify what the page looks like on a failed login.

**Hydra** is a parallelized login cracker which supports numerous protocols to attack.

hydra -l admin -P /root/Desktop/wordlists/rockyou.txt .....ip...  http-post-form '/admin/:user=^USER^&pass=^PASS^:Username or password invalid'

```
hydra -l <Login_Name> -P <wordlist> 10.10.145.133 http-post-form '/admin/:user=^USER^&pass=^PASS^:Username or password invalid'
```

**\-l:** login with LOGIN name

**\-P:** load several passwords from FILE

**\[machine IP\]:** the IP address of the target machine

![c3686205eb250c05e267d07738ab95e7.png](resources/c3686205eb250c05e267d07738ab95e7.png)

&nbsp;

**Q2: Crack the RSA key you found. What is John’s RSA Private Key passphrase?**

Do you remember which ports are open as a result of the nmap scan? When we looked at the nmap scan result, we found that the **SSH** service was running on port **22**. Maybe we can use the private key for ssh connection.

<img alt="" class="bg lm nr c jop-noMdConv" width="700" height="218" loading="lazy" role="presentation" src="resources/1_8iy1AR83wWX6d1o35TWWQQ.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 680px; max-width: 100%; height: auto;">

**RSA Private Key**

**RSA key** is a private key based on RSA algorithm. Private Key is used for authentication and a symmetric key exchange.

<img alt="" class="bg lm nr c jop-noMdConv" width="627" height="640" loading="lazy" role="presentation" src="resources/1_anSrG8HbkgCY8OvBHdFaMw.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 627px; max-width: 100%; height: auto;">

**id_rsa**

We can use the **wget** command to download the key to our machine. <span style="color: #151c2b;">Wget on</span> <span style="color: #151c2b;">Linux</span> <span style="color: #151c2b;">systems or Certutil on Windows are useful tools to download files. Ref:https://tryhackme.com/r/room/pythonforcybersecurity</span>

**Wget** is a command-line utility for downloading files from the web.

<img alt="" class="bg lm nr c jop-noMdConv" width="662" height="199" loading="lazy" role="presentation" src="resources/1_UIGrir5IQ2a9jGioB_yZTg.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 662px; max-width: 100%; height: auto;">

**![3ab6a22dbda958dc657cb038f7fdaa10.png](resources/3ab6a22dbda958dc657cb038f7fdaa10.png)**

**wget**

We can use the special John the Ripper tool called **ssh2john.py** which can extract the crackable hash from the RSA private key.

**Ssh2john** is part of John the Reaper suite. This is a script that basically transforms \[RSA/DSA/EC/OPENSSH (SSH private keys)\] private key to john format for later cracking using JtR.

![7a4af0e292fba413547c525ad777d013.png](resources/7a4af0e292fba413547c525ad777d013.png)

![5ab45069d0b78577c2be7a8d587411c2.png](resources/5ab45069d0b78577c2be7a8d587411c2.png)

**ssh2john**

Now we can use **John the Ripper** to crack this hash and extract the SSH private key password.

**John the Ripper** is a popular open source password cracking tool that combines several different cracking programs and runs in both brute force and dictionary attack modes.

![d11daf0a13d928f3835845b6a6cc58f6.png](resources/d11daf0a13d928f3835845b6a6cc58f6.png)

**John the Ripper**

**Q3: user.txt**

Now we can get in using the RSA private key.

The command `chmod 600 id_rsa` is used to set specific permissions on the `id_rsa` file, which is typically an SSH private key file. Here’s a breakdown:

1.  **`chmod`**: This command changes the permissions of a file or directory.
    
2.  **`600`**: These are the new permissions being applied to the file. In this case:
    
    - **6** means the owner of the file has **read and write** permissions.
    - **0** means that the group has **no permissions**.
    - **0** means that others (anyone who is not the owner or in the group) have **no permissions**.
    
    So, `600` sets the file to be **readable and writable only by the file's owner**, with no permissions for anyone else.
    
3.  **`id_rsa`**: This is the file to which you’re applying these permissions. `id_rsa` is commonly the name of an SSH private key file, which should be kept secure.
    

### Why `chmod 600` is Important for `id_rsa`

An SSH private key file like `id_rsa` needs to be kept private and secure. If the permissions are too open (e.g., readable by others), SSH may refuse to use the key for authentication, or worse, unauthorized users could potentially access it.

By setting permissions to `600`, only the file owner can read or modify `id_rsa`, ensuring it remains private.

![fbdcc01c7cb969f824a51c769a67bba0.png](resources/fbdcc01c7cb969f824a51c769a67bba0.png)

![1cd4e85b4e6f0302a4eaa04fd3ef89e2.png](resources/1cd4e85b4e6f0302a4eaa04fd3ef89e2.png)

**Q4:** Web flag

**<img alt="" class="bg lm nr c jop-noMdConv" width="700" height="218" loading="lazy" role="presentation" src="resources/1_ndq2f-4XnD-348rSCma1Yg.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 680px; max-width: 100%; height: auto;">**

&nbsp;

### **\========================================================================================**

### **Task 4: Privilege Escalation**

Now, we need to escalate our privileges.

**Q1: Find a form to escalate your privileges. What is the root’s password?**

Sudo (Super User DO) command in Linux is generally used as a prefix of some command that only superuser are allowed to run.

sudo -l : The -l (list) option will print out the commands allowed (and forbidden) the user on the current host.

### <img alt="" class="bg lm nr c jop-noMdConv" width="700" height="167" loading="eager" role="presentation" src="resources/1_SQCA7-eL69J9AioHBE97dw.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 680px; max-width: 100%; height: auto;">

### **/bin/cat**

### Google search time!

### <img alt="" class="bg lm nr c jop-noMdConv" width="700" height="272" loading="lazy" role="presentation" src="resources/1__HhRKAHG02JOAWgFQZINqQ.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 680px; max-width: 100%; height: auto;">

### **GTFOBins**

### **GTFOBins** is a curated list of Unix binaries that can be used to bypass local security restrictions in misconfigured systems.

<ins>**https://gtfobins.github.io/gtfobins/cat/#sudo**</ins>

### <img alt="" class="bg lm nr c jop-noMdConv" width="700" height="175" loading="lazy" role="presentation" src="resources/1_JGCjUtupf3uaD-O7dx1WmA.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 680px; max-width: 100%; height: auto;">

### **<span style="color: #242424;">According to GTFOBins, we can use this binary to read files on the system, and since we can run this as root, we can read any file on the system!</span>**

### **A shadow password file**, also known as **/etc/shadow**, is a system file in Linux that stores encrypted user passwords and is accessible only to the root user, preventing unauthorized users or malicious actors from breaking into the system.

- `/etc/shadow` contains **hashed passwords** for system users.
- Normally, only the root user can read this file because it contains sensitive authentication data.
- If `sudo` allows you to read it, you can potentially crack the hashed passwords using **John the Ripper** or **Hashcat**.

![48e4e5764d14b5bef268f9b398cd88d8.png](resources/48e4e5764d14b5bef268f9b398cd88d8.png)

### <img alt="" class="bg lm nr c jop-noMdConv" width="700" height="303" loading="lazy" role="presentation" src="resources/1_YGh8ZahlO1PdHVro3qTv9w.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 680px; max-width: 100%; height: auto;">

### **![ec379ba10c2d573cd470430829b53079.png](resources/ec379ba10c2d573cd470430829b53079.png)**

### **/etc/shadow**

### This time we can crack the root user’s hash using john the ripper.

### ![7dfdab11ee854aa175f76cac5dd95127.png](resources/7dfdab11ee854aa175f76cac5dd95127.png)

### **Q2:** root.txt

The command `su root` is used to switch the current user to the **root** user (the system’s superuser) in Unix-based systems like Linux. Here’s a breakdown of what each part does:

1.  **`su`**: This stands for "substitute user" or "switch user." The `su` command is used to switch from the current user to another user account, usually to perform tasks that require different permissions.
    
2.  **`root`**: This specifies the user account you want to switch to. In this case, `root` is the superuser account, which has unrestricted access to all commands, files, and system configurations on the operating system.
    

### How `su root` Works

When you run `su root`, the system prompts you for the **root user’s password**. Once entered correctly, the session switches to the root user, and you gain administrative privileges. This allows you to perform high-level tasks such as installing software, configuring system files, and managing users.

### Security and Caution

Switching to the root user should be done carefully, as commands run with root privileges can potentially impact the entire system. A mistake, such as deleting or modifying critical files, could lead to system instability or data loss.

### Alternative

Instead of `su root`, many users prefer using `sudo` for individual commands, as it temporarily grants elevated privileges without fully switching to the root account. For example:

`sudo command_to_run_as_root`

This is often considered safer because it limits the duration and scope of root access.

### **su** is an acronym for **switch user** or **substitute user**. You are basically switching to a particular user and you need the password for the user you are switching to. Most often, the user account you switch to is the root account but it can be any account on the system.

### <img alt="" class="bg lm nr c jop-noMdConv" width="579" height="204" loading="lazy" role="presentation" src="resources/1_7wzSdbGfw8qybbNElZq_dg.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 579px; max-width: 100%; height: auto;">

### **root.txt**

### **Resources:**

> ### <ins>**https://nmap.org**</ins>
> 
> ### <ins>**https://www.ssh.com/academy/ssh/openssh**</ins>
> 
> ### <ins>**https://www.kali.org/tools/gobuster**</ins>
> 
> ### <ins>**https://infinitelogins.com/2020/02/22/how-to-brute-force-websites-using-hydra**</ins>
> 
> ### <ins>**https://rapidapi.com/blog/api-glossary/post**</ins>
> 
> ### <ins>**https://www.kali.org/tools/hydra**</ins>
> 
> ### <ins>**https://www.techtarget.com/whatis/definition/John-the-Ripper**</ins>
> 
> ### <ins>**[https://www.geeksforgeeks.org/sudo-command-in-linux-with-examples](https://www.geeksforgeeks.org/sudo-command-in-linux-with-examples "Ctrl-click to open: https://www.geeksforgeeks.org/sudo-command-in-linux-with-examples")**</ins>
> 
> ### <ins>**https://gtfobins.github.io/gtfobins/cat/#sudo**</ins>
> 
> ### <ins>**https://www.techtarget.com/searchsecurity/definition/shadow-password-file**</ins>
> 
> ### <ins>**https://www.redhat.com/sysadmin/difference-between-sudo-su**</ins>
