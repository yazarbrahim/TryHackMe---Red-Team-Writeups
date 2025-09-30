THM Link: https://tryhackme.com/room/easyctf

Access your local kali with openvpn

![5a98d83d88501da7acfca9041d698655.png](../../../_resources/5a98d83d88501da7acfca9041d698655.png)

Let’s kick off the room with a scan I do on every room, nmap.

nmap -p- -sC -sV 10.10.186.250 -vvv -oA nmap_ful --

![b75780ba4f235c2b7c461d4d26d346f5.png](../../../_resources/b75780ba4f235c2b7c461d4d26d346f5.png)

From our results, we can see ports 21 (FTP), 80 (HTTP), and 2222 (SSH) are open.

***Q: How many services are running under port 1000?***

nmap -sV -p 1-1000 10.10.286.250

nmap -sV --top-ports 1000 10.10.186.250

![7a9f9ee107a73341052fdeb76c947117.png](../../../_resources/7a9f9ee107a73341052fdeb76c947117.png)

**Answer:** 2

***Q: What is running on the higher port?***

***![4714d6d737fbded2d8bec18b0b6b3d70.png](../../../_resources/4714d6d737fbded2d8bec18b0b6b3d70.png)***

**Answer**: SSH

**Q3- What's the CVE you're using against the application?**

Knowing there is a website being hosted let’s check it out for any additional information.

First, let’s just browse to the IP and see what we get.

![3643eea7fd7d3b4bd5fbc221753eb2d1.png](../../../_resources/3643eea7fd7d3b4bd5fbc221753eb2d1.png)

Webpage port 80

We find it is the default Apache2 page, not much more to go off of here.

Next, we can use “gobuster” to scan the website for any additional pages.

or dirb

![74701ff48327630f7a9160f77aadb552.png](../../../_resources/74701ff48327630f7a9160f77aadb552.png)

<img alt="" class="bh kp lu c jop-noMdConv" width="700" height="278" loading="lazy" role="presentation" src="../../../_resources/1_Q5zqBviyuk2OOReYTIb92Q.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 680px; max-width: 100%; height: auto;">

“gobuster” results

Using the medium wordlist we supplied, gobuster was able to find there is a webpage at “/simple”. Let’s try browsing to it now and see what we find.

<img alt="" class="bh kp lu c jop-noMdConv" width="700" height="713" loading="lazy" role="presentation" src="../../../_resources/1_D7QFRNYA1sV0Ak3nwn2jRQ.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 680px; max-width: 100%; height: auto;">

/simple webpage

Here we can see this is a default page for something called “CMS Made Simple” and if we look in the bottom corner we can see it is version 2.2.8.

Let’s see if there is anything online about this particular version by simply going to Google and searching “CMS Made Simple 2.2.8 exploit”.

In our results, we see a page on Exploit-DB that matches our search and refers to a SQL injection attack utilizing CVE-2019–9053.

<img alt="" class="bh kp lu c jop-noMdConv" width="700" height="388" loading="lazy" role="presentation" src="../../../_resources/1_MJo7Ktvlh4uMdVxdqLeJ3Q.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 680px; max-width: 100%; height: auto;">

Exploit-DB

**\------------------------------------------------------------------------------------**

**CVE** stands for **Common Vulnerabilities and Exposures**

CVE-2019-9053 is a security vulnerability found in **phpMyAdmin**, a popular web-based administration tool for managing MySQL and MariaDB databases. The vulnerability is classified as a **time-based blind SQL injection** issue

### Description:

The vulnerability occurs in the `designer` feature of phpMyAdmin. An attacker could exploit the vulnerability by sending specially crafted requests that include malicious SQL queries, leading to a time-based blind SQL injection attack.

In a **time-based blind SQL injection**, the attacker injects a SQL query and relies on how long it takes the server to respond to infer information about the database (e.g., the existence of a record, the content of a particular field). The attacker uses this time delay to extract sensitive data from the database, such as usernames, passwords, or other confidential information, without directly seeing the database output.

### Impact:

- The attacker could gain unauthorized access to the database or manipulate the database by exploiting the SQL injection vulnerability.
- If successfully exploited, the attacker could steal sensitive information or escalate privileges.

### Exploitation:

This vulnerability is generally exploited via an authenticated phpMyAdmin user. Once authenticated, an attacker can use the vulnerable part of the `designer` feature to perform SQL injection attacks, potentially compromising the database.

&nbsp;-----------------------------------------------------------------------------------------

***Q: What’s the CVE you’re using against the application?***

**Answer:** CVE-2019-9053

***Q: To what kind of vulnerability is the application vulnerable?***

***![d0626436684e09413a6dca91dd0d4cd2.png](../../../_resources/d0626436684e09413a6dca91dd0d4cd2.png)***

**Answer:** SQLi

**Q: What's the password?**

<span style="color: #242424;">Let’s download the script by using</span> ***wget***<span style="color: #242424;">:</span>

<span style="color: #242424;">also you can download on webpage</span>

***wget*** <ins>***https://www.exploit-db.com/download/46635***</ins>

![1558106a119c153f2f358d42c6b19821.png](../../../_resources/1558106a119c153f2f358d42c6b19821.png)

<span style="color: #6b6b6b;">Trying to run the exploit.py script</span>

python3 46635.py

![74d1d3679a9a95e0964d13a420005ce2.png](../../../_resources/74d1d3679a9a95e0964d13a420005ce2.png)

it did not work we need to convert python2 code python3 you can use AI

Now we know the ports open on the target, an idea of what the target is running on their website, and a possible exploit that we may use.

**The exploit is a python script so I copied and pasted into a .py document.**

Exploit options

Here we can see we need to supply a URL using the -u flag and can supply a wordlist for password cracking using — crack -w.

<img alt="" class="bh kp lu c jop-noMdConv" width="700" height="41" loading="lazy" role="presentation" src="../../../_resources/1_vcwsWYmuEz4GxrsHhMGYsg.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 680px; max-width: 100%; height: auto;">

Exploit syntax

Now, let’s run it and see what we get in return.

![ac29bb7d32a4eee26cbe587d12bfe5ab.png](../../../_resources/ac29bb7d32a4eee26cbe587d12bfe5ab.png)

*(Note: Initially the script would not run due to my machine not having “termcolor” module installed for Python. To fix:* `pip install termcolor`*)*

![ff0cd918a78e28deea4ed4f86d45514a.png](../../../_resources/ff0cd918a78e28deea4ed4f86d45514a.png)

Exploit results

we got username and hash password we can crack this hash with hashcat

\[+\] Salt for password found: 1dac0d92e9fa6bb2  
\[+\] Username found: mitch  
\[+\] Email found: adminh  
\[\*\]  0c01f4468bd75d7a84c7eb73846e8d96

hash-identifier

![cc829f8ba0dd372caa54a42b3236ba76.png](../../../_resources/cc829f8ba0dd372caa54a42b3236ba76.png)

find hash code ; https://hashcat.net/wiki/doku.php?id=example_hashes

&nbsp;

![4a11e20bb198b54f8e0e0b49380d01f4.png](../../../_resources/4a11e20bb198b54f8e0e0b49380d01f4.png)

&nbsp;now hashcat -m 20 0c01f4468bd75d7a84c7eb73846e8d96:1dac0d92e9fa6bb2 -w '/home/kali/Desktop/rockyou.txt'

hashcat -m 20 0c01f4468bd75d7a84c7eb73846e8d96:1dac0d92e9fa6bb2 '/home/kali/Desktop/rockyou.txt'

![c1175f0a0bba170367428be0304c5575.png](../../../_resources/c1175f0a0bba170367428be0304c5575.png)

Bingo! We got a username and a cracked password returned from the exploit.

Second way --> just do Hydra

![814a2b82edb7db599ab94675466faca6.png](../../../_resources/814a2b82edb7db599ab94675466faca6.png)

***Q; What’s the password?***

**Answer:** secret

***Q: Where can you login with the details obtained?***

**Answer:** SSH

Using the username and password we discovered we can now try to SSH into the target machine.

Username found: mitch

password: secret

![abedb77f4c0e17c7dcc1b70a09f74118.png](../../../_resources/abedb77f4c0e17c7dcc1b70a09f74118.png)

we need to specified port number otherwise it is not working

ssh mitch@10.10.133.14 -p 2222

ssh mitch@10.10.133.14 -p 2222 why we need specified port number

\--> In the command `ssh mitch@10.10.133.14 -p 2222`, the `-p 2222` specifies the port number that the SSH client should connect to. By default, SSH uses **port 22** for communication. However, the specified port `2222` in this command indicates that the **SSH server is running on a non-default port**.

\--> Specifying the port number with `-p 2222` is necessary when the SSH service is configured to run on a port other than the default port 22. Without specifying the port, the SSH client would attempt to connect to port 22, and the connection would fail if the SSH service is running on a different port, such as 2222.

&nbsp;

![dedd0a6db616f2af12ed79c5e737875c.png](../../../_resources/dedd0a6db616f2af12ed79c5e737875c.png)

ssh

Now if we ls we see the “user.txt” file and read it for our first flag!

![d68d66037288df86068324a2ec86ba81.png](../../../_resources/d68d66037288df86068324a2ec86ba81.png)

user flag

***Q: What’s the user flag?***

**Answer:** user.txt

Next let’s check if any other users have home directories.

![2f9410321f9e4ae5b05fc37deed81d00.png](../../../_resources/2f9410321f9e4ae5b05fc37deed81d00.png)

Other users

***Q: Is there any other user in the home directory? What’s its name?***

**Answer:** sunbath

We can open root because we are not root privilege

![2c3bcb41c84efa1e46e89339ff96f940.png](../../../_resources/2c3bcb41c84efa1e46e89339ff96f940.png)

![25cf757c812b4aeec4af2149791fda2f.png](../../../_resources/25cf757c812b4aeec4af2149791fda2f.png)mitch

On to privileged escalation! First I like to start off with running “sudo -l” to see what my current user can run.

The \`sudo -l\` command is used in Unix-like operating systems to list the allowed and forbidden commands for the current user or a specified user when using \`sudo\`.

When you run \`sudo -l\`, it will show you:

1\. The commands that you can run with \`sudo\`.  
2\. Any restrictions or limitations on those commands.  
3\. The user's privileges in terms of executing commands as other users or the root user.

![d1965c64f750c709eed17df684c5fa37.png](../../../_resources/d1965c64f750c709eed17df684c5fa37.png)

sudo -l

\--> Sudo vim info https://gtfobins.github.io/gtfobins/vim/#sudo

\--> https://github.com/Mahamedm/CVE-2019-9053-Exploit-Python-3

&nbsp;

We can see the user “mitch” can run /usr/bin/vim without a password. With that information, let’s check out GTFOBins and see if we can use that for privesc.

<img alt="" class="bh kp lu c jop-noMdConv" width="700" height="318" loading="lazy" role="presentation" src="../../../_resources/1_zHgbmpz67-UND7bOUzI69w.png" style="box-sizing: inherit; vertical-align: middle; background-color: #ffffff; width: 680px; max-width: 100%; height: auto;">

GTFOBins

Looks like if we run this command here we can escalate our privileges!

The command \`sudo vim -c ':!/bin/sh'\` does the following:

1\. \*\*\`sudo\`\*\*: Runs the command with superuser (root) privileges.  
2\. \*\*\`vim\`\*\*: Opens the Vim text editor.  
3\. \*\*\`-c ':!/bin/sh'\`\*\*: The \`-c\` option allows you to run a command after starting Vim. Here, \`:!/bin/sh\` executes a shell command, specifically launching a new shell (\`/bin/sh\`).

When you run this command, Vim starts, and immediately a new shell is executed. You'll effectively be dropped into a shell session within Vim. To exit the shell and return to Vim, you can type \`exit\` or press \`Ctrl+D\`.

\--> If the `sudo -l` command reveals that a user can run `vim` with root privileges (i.e., `sudo vim` is allowed without needing a password), the attacker can abuse this privilege to escalate to root. In this case, `vim -c ':!/bin/sh'` opens `vim` and immediately runs the shell command `!/bin/sh`, which spawns a shell. Since it is executed with `sudo`, this will be a root shell.

This approach can be useful for running shell commands with root privileges while in a text editing environment.

***Q: What can you leverage to spawn a privileged shell?***

**Answer:** vim

![680722ce6705c0ca32f5b2b194bfbef6.png](../../../_resources/680722ce6705c0ca32f5b2b194bfbef6.png)

root shell

It worked! From here all we have left is to grab the root flag and the room is complete.

![7546d7396128a5613b335a3b1fd33f09.png](../../../_resources/7546d7396128a5613b335a3b1fd33f09.png)

root.txt

***Q: What’s the root flag?***

**Answer:** root.txt