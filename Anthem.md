Link --> https://tryhackme.com/room/anthem

<span style="color: #151c2b;">This task involves you, paying attention to details and finding the 'keys to the castle'.</span>

<span style="color: #000000;">**<span><span>Task 1:</span> <span>Website Analysis</span></span>**</span>

**<span style="color: #000000;">Let's run nmap and check what ports are open.</span>**

nmap -A -p- -Pn -T5 10.201.1.210 > nmap.txt

**What port is for the web server?**

![d47e52815f7c1f8caa301dd60030ee67.png](../resources/d47e52815f7c1f8caa301dd60030ee67.png)

**What port is for remote desktop service?**

80

**What is a possible password in one of the pages web crawlers check for?**

**![340af0cb9a58e984576e242fd830ff93.png](../resources/340af0cb9a58e984576e242fd830ff93.png)**

Let's started Enumeration and Exploitation

As per the hint to get the password, We have a populer two .txt file. Who can say what are those?  
Robots.txt and Rockyou.txt. right now we will use robots.txt. We visited the robots.txt file and found a potential string that could be the password.

![d6a53d9488c13ff8ca782626520453f7.png](../resources/d6a53d9488c13ff8ca782626520453f7.png)

&nbsp;

**What CMS is the website using?**

nmap -p 80 --script http-enum 10.201.71.182

![31c97be775f04cc95203b41468ab8a66.png](../resources/31c97be775f04cc95203b41468ab8a66.png)

curl http://10.201.71.182/robots.txt

![8005917438b6b13af1be6ea5d6e07d74.png](../resources/8005917438b6b13af1be6ea5d6e07d74.png)

**![5acd48d6bef22e80ce6145ba60890bbb.png](../resources/5acd48d6bef22e80ce6145ba60890bbb.png)**

A **CMS (Content Management System)** is a software platform that allows users to create, manage, and modify digital content without needing extensive technical knowledge. It simplifies website development by providing a user-friendly interface, templates, and plugins for customization. Popular CMS platforms include **WordPress, Joomla, Drupal, and Umbraco**.

### What is Umbraco?

**Umbraco** is an **open-source CMS** built on the **.NET framework** and primarily uses **C#** for backend development. It is known for being highly flexible, developer-friendly, and ideal for businesses that require customized solutions.

### How Can a Pentester Exploit Umbraco?

Since Umbraco is a widely used CMS, pentesters often check for **security misconfigurations and vulnerabilities**, including:

1.  **Default Credentials** – Some outdated installations use `admin` / `default` login pairs.
2.  **Version Disclosure** – Checking `/umbraco/Config/` can reveal the version, which helps find known exploits.
3.  **Exposed Admin Panel** – The login panel is typically at `/umbraco/`, which can be brute-forced.
4.  **SQL Injection (SQLi)** – Some older versions had vulnerabilities that allowed SQLi attacks.
5.  **Remote Code Execution (RCE)** – Exploits exist for outdated versions that allow attackers to execute

curl -s https://target.com/umbraco/

**What is the domain of the website?**

![5071ca3d7da3c605d4b22bd75add1e94.png](../resources/5071ca3d7da3c605d4b22bd75add1e94.png)

&nbsp;

**What's the name of the Administrator**

![7a319f8b7fb03b93f8b80e7ce5c482e4.png](../resources/7a319f8b7fb03b93f8b80e7ce5c482e4.png)

![175161d470e2e763d8deee0ed6f66e6f.png](../resources/175161d470e2e763d8deee0ed6f66e6f.png)

![f3c6480928332e0b33253a83bee393cb.png](../resources/f3c6480928332e0b33253a83bee393cb.png)

&nbsp;

**Can we find find the email address of the administrator?**

![e8ceb4600a5b5463bf5b8727133e442b.png](../resources/e8ceb4600a5b5463bf5b8727133e442b.png)

based on above information the Administartor email could be sg@anthem.com

**<span style="color: #000000;"><span>Task 2 --></span> <span>Spot the flags</span></span>**

**Anthem are hiring!**

**What is flag 1?**

**![aa9e7534db920d52f1c20a611d52e81f.png](../resources/aa9e7534db920d52f1c20a611d52e81f.png)**

**![8df807703b78030e29ff1bda778131a5.png](../resources/8df807703b78030e29ff1bda778131a5.png)**

**![48af46b6cf36195b95d884fd15519092.png](../resources/48af46b6cf36195b95d884fd15519092.png)**

**What is flag 2?**

![3d4718a78bef05d48a76b877af678aab.png](../resources/3d4718a78bef05d48a76b877af678aab.png)

or in main page

![0d7e14747ac0b2ae1d763e482bb40b0d.png](../resources/0d7e14747ac0b2ae1d763e482bb40b0d.png)

&nbsp;

**What is flag 3?**

**![735927ab7668cf34fdb20b31fa3d20a5.png](../resources/735927ab7668cf34fdb20b31fa3d20a5.png)**

&nbsp;

**What is flag 4?**

![fa353027658d30ad6d184f7f5f120491.png](../resources/fa353027658d30ad6d184f7f5f120491.png)

![b4b6962f47d43ecdf971db667d37be2a.png](../resources/b4b6962f47d43ecdf971db667d37be2a.png)

![b5c76afdd095bc1f00fef04a73ee2e8c.png](../resources/b5c76afdd095bc1f00fef04a73ee2e8c.png)

**<span style="color: #000000;"><span>Task 3 --></span> <span>Final stage</span></span>**

**Let's figure out the username and password to log in to the box.(The box is not on a domain)**

We did not find any admin or login page so we can not do http hydra but on nmap scan we noticed that 3389/tcp which is RDP and we can connect with rdp

**Gain initial access to the machine, what is the contents of user.txt?**

Lets do RDP

xfreerdp /v:10.10.84.120 /u:sg /p:UmbracoIsTheBest! /dynamic-resolution

`/v` = "virtual desktop server" or "remote target"

you can do without /dynamic-resolution

**`/dynamic-resolution`** → Enables **dynamic resolution scaling**, which automatically adjusts the screen resolution based on the size of the RDP window.

**![06380725c3dc70d4112b08830dbebae3.png](../resources/06380725c3dc70d4112b08830dbebae3.png)**

![4273a0df77f174cb1b3a9024aa5dce33.png](../resources/4273a0df77f174cb1b3a9024aa5dce33.png)

![9e4f838929d390581b14c8363e002f1b.png](../resources/9e4f838929d390581b14c8363e002f1b.png)

**Can we spot the admin password? (It is hidden.)**

how to find hidden files on cmd

To find **hidden files** using the Windows Command Prompt (CMD), you can use the **`dir`** command with the `/A` (attribute) option.

### 1️⃣ **Show Hidden Files in a Directory**

`dir /A:H`

- **`/A:H`** → Shows files with the **Hidden** attribute in the current directory.

### 2️⃣ **Show Hidden Files and Folders with Details**

`dir /A:H /S`

- **`/S`** → Searches **all subdirectories** for hidden files.

### 3️⃣ **Show All Files, Including Hidden & System Files**

`dir /A`

- This lists **all** files, including **hidden (H)** and **system (S)** files.

### 4️⃣ **Find Hidden Files in a Specific Directory**

`dir C:\Users\YourUsername\Desktop /A:H`

dir /a:hd

- Replace **`C:\Users\YourUsername\Desktop`** with the path where you want to search.
- This lists only files with the **Hidden (H)** attribute.

![b330b4babcde3a89125ab6130e9d2a07.png](../resources/b330b4babcde3a89125ab6130e9d2a07.png)

![21d89a843ded3b81e54e36db95b7693c.png](../resources/21d89a843ded3b81e54e36db95b7693c.png)

![c37fc87a2559c84e4b5b918910abf10b.png](../resources/c37fc87a2559c84e4b5b918910abf10b.png)

Let’s keep exploring the file system.

I started to use command prompt to navigate around the system. I was also going to search for hidden files, as the hint itself is “It Is Hidden”.

We can use command prompt to show hidden files. If the permissions are weak, we will be able to access it too.

![ad95af37187520d1f3cda9833156350e.png](../resources/ad95af37187520d1f3cda9833156350e.png)

![58ce978407165f19d6d9557f62b03eca.png](../resources/58ce978407165f19d6d9557f62b03eca.png)

So we don’t have permission to view it, but I mentioned earlier that poor permissions can help us… maybe we can change those permissions?

There are two ways we can do this. We can use the CLI or GUI. I will showcase both. Overtime, CLI becomes the quicker way — but the GUI will always be ‘easier’.

However, it seems CLI may not be possible?

So with that in mind, GUI it is.

Let’s enable hidden files to be seen in the GUI

![f5b8b7ca8665cf1113e1b18da82f850e.png](../resources/f5b8b7ca8665cf1113e1b18da82f850e.png)

![b1e15ddd209be9261c43ab64e0f0dfd9.png](../resources/b1e15ddd209be9261c43ab64e0f0dfd9.png)

we got permission issue

![c802995faff28e99bc9b6d67a6ba70f3.png](../resources/c802995faff28e99bc9b6d67a6ba70f3.png)

![d67df333133e6aaae0fca70cde43594d.png](../resources/d67df333133e6aaae0fca70cde43594d.png)

We need to add our user to this list. We can find our user by clicking ‘Advanced’.

![278e201de7db80b8974a8d73ccf108e6.png](../resources/278e201de7db80b8974a8d73ccf108e6.png)

![38c959e95efd8a52ca8c5f594ecd24c1.png](../resources/38c959e95efd8a52ca8c5f594ecd24c1.png)

![a9f3a9a59a3d4902cc7066d79f218b4a.png](../resources/a9f3a9a59a3d4902cc7066d79f218b4a.png)

Make sure you click apply!

![fe614a81bbd4d2537f2dc78d66d0bff3.png](../resources/fe614a81bbd4d2537f2dc78d66d0bff3.png)

&nbsp;

**Escalate your privileges to root, what is the contents of root.txt?**

**![0d4784d466606c24e04c6fe65eacc922.png](../resources/0d4784d466606c24e04c6fe65eacc922.png)**

**With Command prompt: below command we can use for the escalate the our privileges.**

**runas /user:Administrator cmd**

**![83854693bc1d6aa6ce2241f8fe82e04d.png](../resources/83854693bc1d6aa6ce2241f8fe82e04d.png)**

**![bfc70a4bbb8f7af89ff95161298bfb7a.png](../resources/bfc70a4bbb8f7af89ff95161298bfb7a.png)**

**![343d3162a192b78bda13b3d7b0d67e4a.png](../resources/343d3162a192b78bda13b3d7b0d67e4a.png)**

Second way:

xfreerdp /v:10.201.9.2 /u:Administrator /p:ChangeMeBaby1MoreTime

**![5084a05a5eddf14186bf7d8ff5292589.png](../resources/5084a05a5eddf14186bf7d8ff5292589.png)**

![447a28b7a7be843883124468b657d1c9.png](../resources/447a28b7a7be843883124468b657d1c9.png)

Third Way

From SG machine

![2e06a3d4a9c2b1a0080da6f291e1737f.png](../resources/2e06a3d4a9c2b1a0080da6f291e1737f.png)

.![19ce04b948f31149d8a6f9fc4958d8e7.png](../resources/19ce04b948f31149d8a6f9fc4958d8e7.png)

![f4498eeac5faa614c298a464b81a5382.png](../resources/f4498eeac5faa614c298a464b81a5382.png)

and login with password

![8f12217ffef00bc6bea8015d42ceb7f9.png](../resources/8f12217ffef00bc6bea8015d42ceb7f9.png)

![d6c6ee0a1a5e5e797ca08492efc3c612.png](../resources/d6c6ee0a1a5e5e797ca08492efc3c612.png)
