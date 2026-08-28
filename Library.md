https://tryhackme.com/r/room/bsidesgtlibrary

Objectives:

1==> Understanding of SSH and brute-forcing

2==> Using Python for privilege Escalation to play around with a low-privileged user role.

**Enumeration - Reconnaissance**

nmap  -p- -sC -sV (IP) -vvv -oA nmap_full ​

We find 2 open ports; one is 22 ssh and other one is 80 http that means a **webpage is running** on port 80.

![4d4c8c9c4c87737e5d87a8ee49662a33.png]( resources/4d4c8c9c4c87737e5d87a8ee49662a33.png)

![4ed6c69f363c40412b2f15b85554669d.png]( resources/4ed6c69f363c40412b2f15b85554669d.png)

![059db079a341c014a7fa4ea9fbac4fc1.png]( resources/059db079a341c014a7fa4ea9fbac4fc1.png)

![616dba04516e250122e735ac594c6cf7.png]( resources/616dba04516e250122e735ac594c6cf7.png)


Let's check out the HTTP webpage

![c03896e5634bbdd24586995bdc55311a.png]( resources/c03896e5634bbdd24586995bdc55311a.png)

We find the user --> Meliodas, and we can do brute force to find the password for this user and can access he/she account

Robots.txt --> https://tryhackme.com/r/room/contentdiscovery

Robots.txt

The robots.txt file is a document that tells search engines which pages they are and aren't allowed to show on their search engine results or ban specific search engines from crawling the website altogether. It can be common practice to restrict certain website areas so they aren't displayed in search engine results. These pages may be areas such as administration portals or files meant for the website's customers. This file gives us a great list of locations on the website that the owners don't want us to discover as penetration testers.

hydra -l meliodas -P /root/Desktop/wordlists/rockyou.txt ssh://10.10.36.165

![00906a0f7481f6bf0bef14080911fd51.png]( resources/00906a0f7481f6bf0bef14080911fd51.png)

We find the password for user meliodas is iloveyou1; now we can log in with these user credentials

ssh meliodas@10.10.36.165

![5a6fe504d6e9c6d8c00d7cf3a9b4ac7b.png]( resources/5a6fe504d6e9c6d8c00d7cf3a9b4ac7b.png)

Let's find the user.txt by looking around and found the first flag, i.e., user.txt.

![9267035770887f184fee6b92ec568984.png]( resources/9267035770887f184fee6b92ec568984.png)

cat

We got the user flag and completed the first part of our task, which was to get the user flag.

![0d4e4360bbdf093c3d8175360997dcc4.png]( resources/0d4e4360bbdf093c3d8175360997dcc4.png)

> user flag:- **6d488cbb3f111d135722c33cb635f4ec**

# **Privilege escalation**

Checking the permissions we have.

find / -type f -name root.txt

&nbsp;

> Command :- sudo -l   ==> is used in Linux to list the `sudo` privileges of the current user
> 
> - It can also list whether the user is allowed to run commands without a password prompt.
>     
> - ### Breakdown:
>     
>     1.  **User `meliodas`**:
>         
>         - This is the username that has specific `sudo` privileges on the system.
>     2.  **`(ALL)`**:
>         
>         - This indicates that the user `meliodas` can execute the specified commands as any user, including root, or any other user on the system.
>     3.  **`NOPASSWD:`**:
>         
>         - This means that when `meliodas` runs the specified commands with `sudo`, they will not be prompted to enter their password. Normally, `sudo` requires a password, but with `NOPASSWD`, this requirement is bypassed for the commands listed.
>     4.  **`/usr/bin/python*`**:
>         
>         - This allows `meliodas` to run any version or variant of the `python` executable located in `/usr/bin/` with `sudo`. The `*` is a wildcard, meaning it matches any files that start with `python` (like `python`, `python3`, `python2.7`, etc.).
  5.  **`/home/meliodas/bak.py`**:
        
 - This specifies that `meliodas` can run the script `/home/meliodas/bak.py` with `sudo` without being prompted for a password.


This command would run the `bak.py` script using Python 3 with elevated privileges, without requiring `meliodas` to enter their password.

We see that user meliodas can run a file called bak.py using Python, but when I tried to run it, it gave an error and said permission was denied.

![3ebdfa9508921290038f7e587803c0de.png]( resources/3ebdfa9508921290038f7e587803c0de.png)

![cb3279326f2d7a0093fc735b05e3b81e.png]( resources/cb3279326f2d7a0093fc735b05e3b81e.png)

So, I deleted the file & re-create it with spawn using Python. And then ran that bak.py again, which gave me **root privileges.**

**"spawn" generally refers to the creation or execution of a new process** or **task**

**echo 'import pty; pty.spawn("/bin/sh")' > /home/meliodas/bak.py**

![91c2d4593e6b1db904a168cb326b7c14.png]( resources/91c2d4593e6b1db904a168cb326b7c14.png)

echo 'import pty; pty.spawn("/bin/sh")' > /home/meliodas/bak.py

![3e6a9a0b685b6f0f909111522d7e7d4b.png]( resources/3e6a9a0b685b6f0f909111522d7e7d4b.png)

The command \`import pty; pty.spawn("/bin/sh")\` is used in a Python environment to spawn a terminal (PTY) and run a shell (\`/bin/sh\`) within it. Here's why and when you might use it

How it Works  
\- \`import pty\`: This imports the \`pty\` module in Python, which provides the functionality to start and interact with pseudo-terminals.  
\- \`pty.spawn("/bin/sh")\`: This spawns a new process running \`/bin/sh\` (a shell) within the pseudo-terminal, effectively creating an interactive shell session.

This command is powerful, especially in the context of security and remote administration, but it should be used with caution and only in environments where you have the appropriate permissions.

![83f85d0f730b53d8b3d6e65a04e46be5.png]( resources/83f85d0f730b53d8b3d6e65a04e46be5.png)

Now we will find the root flag.


second way   
use os.system

1.  echo 'import os; os.system("/bin/bash")' > bak.py
    
2.  sudo /usr/bin/python /home/meliodas/bak.py
    

![673476d6e8458cb0a72be0061c69df0e.png]( resources/673476d6e8458cb0a72be0061c69df0e.png)

![9de9090c9449df5ad4eb1fe5b850b853.png]( resources/9de9090c9449df5ad4eb1fe5b850b853.png)

\# cat root.txt  
e8c8c6c256c35515d1d344ee0488c617
