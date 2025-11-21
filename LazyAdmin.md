https://tryhackme.com/room/lazyadmin

**What is the user flag?**

nmap -sV -sC -p- (ip) -vvv

![29bc350b9a5a740f87f8a8249fa28c21.png](resources/29bc350b9a5a740f87f8a8249fa28c21.png)

That page is the **default welcome page** for a **successfully installed and running Apache HTTP Server** on a computer using the **Ubuntu** operating system.

- **Apache is installed:** **Apache** is the name of f **web server software**.
    
- **Apache is working:**
    
- **Ubuntu** (The Operating System) Ubuntu is a very popular operating system that is based on Linux. It's often used for servers because it's stable, secure, and free.
    

![e18cb98c19d4629f6200cbd9cf25c25e.png](resources/e18cb98c19d4629f6200cbd9cf25c25e.png)

Here, we encounter some information indicating that the page is not set up. Let's try gathering more information using the Gobuster tool or Dirb

![46eb3dd3c395dd40701aaecf6fc0a366.png](resources/46eb3dd3c395dd40701aaecf6fc0a366.png)

![acce71ee8ddd80ded7b0ca131f1f5051.png](resources/acce71ee8ddd80ded7b0ca131f1f5051.png)

Lazy admin is still working, he is lazy admin:))))

Unlike the other directories, we can see the /inc and /as directories. Let's proceed with our operations by examining these 2 directories.

![6693b5eb2df6d8c013de19acaa63a843.png](resources/6693b5eb2df6d8c013de19acaa63a843.png)

STEP 5: Content/as

![e563632945da7d27ba4ca195f81c7d0f.png](resources/e563632945da7d27ba4ca195f81c7d0f.png)

In this directory, we encounter a login screen. Let's quickly review the /inc directory to gather more information about this screen.

content/inc

![ce9d491c7c6c6bacb6479c51201fcb98.png](resources/ce9d491c7c6c6bacb6479c51201fcb98.png)

The listed files (`404.php`, `db.php`, `do_attachment.php`, `do_sitemap.php`, etc.) appear to be **PHP script components**. The folder name `/inc` often stands for "**include**" or "**includes**," meaning these are files designed to be included by other main scripts to perform specific functions (like connecting to a database, handling sitemaps, or showing alert messages).

In this directory, there is also a mysql_backup directory where we obtained an SQL file, and we click it

![25036cc56eb1ce860d9753ad786eb6d5.png](resources/25036cc56eb1ce860d9753ad786eb6d5.png)

&nbsp;

![23d240097db232cc4d06317b08c9ce41.png](resources/23d240097db232cc4d06317b08c9ce41.png)

user: manager

42f749ade7f9e195bf475f37a44cafcb

![932fa7023161390b624412e7432ffa44.png](resources/932fa7023161390b624412e7432ffa44.png)

![7014046afa1dd25dbd51a122946e7d76.png](resources/7014046afa1dd25dbd51a122946e7d76.png)

![7e677933e13039e18c62b1e607dac1de.png](resources/7e677933e13039e18c62b1e607dac1de.png)

https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php

Change the local IP and save it

Open your terminal and type nc -lvnp 1234 and enter

Open your browser and click the sell.php file

![cdc1f797cbc32d9df22a23fa7ff97f10.png](resources/cdc1f797cbc32d9df22a23fa7ff97f10.png)

- `nc` = netcat
    
- `-l` = listen mode
    
- `-v` = verbose
    
- `-n` = no DNS resolution
    
- `-p 1234` = listen on port **1234**
    

This command waits for another machine to *connect back* to this port

# **What is the `www-data` user?**

`www-data` is a **low‑privileged service account** used by web servers such as:

- Apache
    
- Nginx
    
- Lighttpd
    
It is *not* a real human login.  
It exists only so the web server can run safely without using root.

![9cc665036e00e20e02240cd6ba61c863.png](resources/9cc665036e00e20e02240cd6ba61c863.png)

![9abea0393aed1830ca99c01a741fea12.png](resources/9abea0393aed1830ca99c01a741fea12.png)

![4a5f11a491eee0caa5c89c53cdd8a619.png](resources/4a5f11a491eee0caa5c89c53cdd8a619.png)

THM{63e5bce9271952aad1113b6f1ac28a07}

Just work around and try to find more information

python -c 'import pty; pty.spawn ("/bin/sh")'

![053ae06844105831351bc5026267cd5a.png](resources/053ae06844105831351bc5026267cd5a.png)

![ef6602300de82d13d758ecd487ca0ca8.png](resources/ef6602300de82d13d758ecd487ca0ca8.png)

![e297e92d4c60a4c1da8b82ced6186f20.png](resources/e297e92d4c60a4c1da8b82ced6186f20.png)

**What is the root flag?**

sudo -l

This command shows what the current user (`www-data`) is allowed to run with `sudo` **without a password**.

![0beac098e8a43b0d0911b3c7bd1fb1b4.png](resources/0beac098e8a43b0d0911b3c7bd1fb1b4.png)

- The `www-data` user is allowed to run:  
    **`/usr/bin/perl /home/itguy/backup.pl`**
    
- It can run it as **any user**, including **root**.
    
    - `/etc/copy.sh` shouldn’t be writable by `www-data`.
        
    - And `backup.pl` shouldn’t be allowed to run by `www-data` as root.
        

![adab0268e46cc842a0b5af82db8a6b83.png](resources/adab0268e46cc842a0b5af82db8a6b83.png)

This means `/etc/copy.sh` contains some kind of script — in this case, something designed to run a shell through a network connection.

What matters for the lab:  
**`backup.pl` probably calls or references `/etc/copy.sh`**, so if you can modify `/etc/copy.sh`, you control what runs as root.

![043cb8b3c4349b26534758696a1b9f7d.png](resources/043cb8b3c4349b26534758696a1b9f7d.png)

# **Replacing the script**

`echo "/bin/bash" > /etc/copy.sh`

This overwrites `/etc/copy.sh` so that it now contains:

`/bin/bash`

So instead of the old code, `/etc/copy.sh` will simply start a bash shell when executed.

Why this matters:  
**If `backup.pl` runs `/etc/copy.sh`, then it will run `/bin/bash` as root.**

* * *

# **Running the vulnerable program with sudo**

`sudo /usr/bin/perl /home/itguy/backup.pl`

Because of the sudo rule, this runs `backup.pl` **as root**, without a password.

Inside `backup.pl`, it likely executes `/etc/copy.sh`.  
Since you replaced `/etc/copy.sh` with “`/bin/bash`”, it launches **a bash shell running as root**.

![5f96e1474df4503ad38715a3a70e23fa.png](resources/5f96e1474df4503ad38715a3a70e23fa.png)
