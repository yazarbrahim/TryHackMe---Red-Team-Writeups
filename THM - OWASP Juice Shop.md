# OWASP Juice Shop

![db8f8a0d5e39b5c159d1d77443e1b5f1.png]( resources/db8f8a0d5e39b5c159d1d77443e1b5f1.png)

**Task 1 : Open for business!**

Within this room, we will look at OWASP’s TOP 10 vulnerabilities in web applications. You will find these in all types of web applications. But for today we will be looking at OWASP’s own creation, Juice Shop!

**#1 Deploy the VM attached to this task to get started! You can access this machine by using your browser-based machine, or if you’re connected through** **OpenVPN**

**#2 Once the machine has loaded, access it by copying and pasting its IP into your browser; if you’re using the browser-based machine, paste the machines IP into a browser on that machine.**

**Task 2 : Let’s go on an adventure!**

**Question #1: What’s the Administrator’s email address?**

We have to go visit website and by clicking on Apple Juice(1000ml) product, we can find out the admin’s email address.

![d17ef36989d9f9ef74dab7f2befd1f89.png]( resources/d17ef36989d9f9ef74dab7f2befd1f89.png)

![846ff339180544c06c738f2a4bc781fe.png]( resources/846ff339180544c06c738f2a4bc781fe.png)

**Answer** : admin@juice-sh.op

**Question #2: What parameter is used for searching?**

Click on the magnifying glass in the top right of the application will pop out a search bar.

![b4373577f5fe0bf7c83419724064e17c.png]( resources/b4373577f5fe0bf7c83419724064e17c.png)

We can then input text and by pressing Enter will search for the text which was just inputted.

![0b19c7e1a20e384386b1e02dfba99eee.png]( resources/0b19c7e1a20e384386b1e02dfba99eee.png)

Now pay attention to the URL which will now update with the text we just entered.

![4a5dc9c5343b937bf99244647be0e027.png]( resources/4a5dc9c5343b937bf99244647be0e027.png)

http://10.67.191.103/#/search?q=a

We can now see the search parameter after the /#/search? the letter q

**Answer** : q

**Question #3: What show does Jim reference in his review?**

Search green smoothie

**![522d2d0d7138ad378fd86ad6a75466a0.png]( resources/522d2d0d7138ad378fd86ad6a75466a0.png)**

Jim did a review on the Green Smoothie product. We can see that he mentions a replicator.

![8360215b9df7a9e2c0dacc4c56cb8be6.png]( resources/8360215b9df7a9e2c0dacc4c56cb8be6.png)

![7e874c6f57207a296feda22da794b17e.png]( resources/7e874c6f57207a296feda22da794b17e.png)

**Answer** : Star Trek

**Task 3: Inject the juice**

This task will be focusing on injection vulnerabilities. Injection vulnerabilities are quite dangerous to a company as they can potentially cause downtime and/or loss of data. Identifying injection points within a web application is usually quite simple, as most of them will return an error. There are many types of injection attacks, some of them are:

**SQL Injection**  
SQL Injection is when an attacker enters a malicious or malformed query to either retrieve or tamper data from a database. And in some cases, log into accounts.  
**Command Injection**  
Command Injection is when web applications take input or user-controlled data and run them as system commands. An attacker may tamper with this data to execute their own system commands. This can be seen in applications that perform misconfigured ping tests.  
**Email Injection**  
Email injection is a security vulnerability that allows malicious users to send email messages without prior authorization by the email server. These occur when the attacker adds extra data to fields, which are not interpreted by the server correctly.

**Question #1: Brute force the Administrator account’s password!**

We need to start burp-suite and enable the Burp on browser

![3d67c0e4fe8afc7467306844a477fde2.png]( resources/3d67c0e4fe8afc7467306844a477fde2.png)

![fe1a8089d0fd527c342c1ba12c5ae8ef.png]( resources/fe1a8089d0fd527c342c1ba12c5ae8ef.png)

You need to enabled the Burb on browser

**Before** clicking submit, make sure **Intercept mode is** **on**

see the post method.

![491bf0c737954a7d69dbe24779e0b7f4.png]( resources/491bf0c737954a7d69dbe24779e0b7f4.png)

Change the email (user) with:  ’ or 1=1 —

![9b7e5b4e1a3c758bf36f106264199ee6.png]( resources/9b7e5b4e1a3c758bf36f106264199ee6.png)

![0f78290558e415e6d9bbd57465fb863e.png]( resources/0f78290558e415e6d9bbd57465fb863e.png)

Click forward

![bc6b7f08c3e1782235eaa12e1307c6d6.png]( resources/bc6b7f08c3e1782235eaa12e1307c6d6.png)

**Answer:** 690fa3247a99d651e0b26f947baf0b79b4f404a9

**Question #2: Log into the Bender account!**

Similar to what we did in Question #1, we will now log into Bender’s account! Capture the login request again, but this time we will put: **bender@juice-sh.op** as the email.

Logout admin account

![670cf3111f406fd3009eafe6a2ff6e76.png]( resources/670cf3111f406fd3009eafe6a2ff6e76.png)

Login Bender account

![d16ceedd41838b296df9d95fc560c26f.png]( resources/d16ceedd41838b296df9d95fc560c26f.png)

make sure Intercept is on and Burp is enable before click login

Click the login and then click the forward

![e56400e11da9eee6bd77bdc65cd80351.png]( resources/e56400e11da9eee6bd77bdc65cd80351.png)

![ac876922f8df5da6b12f1e585b0cc413.png]( resources/ac876922f8df5da6b12f1e585b0cc413.png)

**Answer** :5ff5052e879e6fef64124e64c82c84ebc809c6c4

**Task 4: Who broke my lock?!**

**Question #1: Bruteforce the Administrator account’s password!**

Go to Positions and then select the **Clear §** button. In the password field place two § inside the quotes. To clarify, the § § is not two sperate inputs but rather Burp’s implementation of quotations e.g. “”. The request should look like the image below.

For the payload, we will be using the best1050.txt from Seclists. (Which can be installed via: apt-get install seclists)

*You can load the list from: /usr/share/wordlists/SecLists/Passwords/Common-Credentials/best1050.txt*

Once the file is loaded into Burp, start the attack. You will want to filter for the request by status.

A **failed** request will receive a **401 Unauthorized**

Whereas a **successful** request will return a **200 OK**.

Once completed, login to the account with the password.

#send the request to intruder.

Select the password and click Add.

Logout he account

![53463743c73116a71cf4c88d42345e5c.png]( resources/53463743c73116a71cf4c88d42345e5c.png)

![ffb49193e68bddd3b798599608a983ca.png]( resources/ffb49193e68bddd3b798599608a983ca.png)

![e2a5f204ae48bfe5fc222995fd78bd0c.png]( resources/e2a5f204ae48bfe5fc222995fd78bd0c.png)

![59dc45a51d5d4af433e2bf2eb2ada2dc.png]( resources/59dc45a51d5d4af433e2bf2eb2ada2dc.png)

![d18f50a2521211cb65139cae5272e573.png]( resources/d18f50a2521211cb65139cae5272e573.png)

![2f247f766273651ec61a3b97d3de16a4.png]( resources/2f247f766273651ec61a3b97d3de16a4.png)

![02249b5f32bdf448a8321ce7c1eda1cc.png]( resources/02249b5f32bdf448a8321ce7c1eda1cc.png)

Click the payload  
![2f68bae7387c98d95271124a8d7351a6.png]( resources/2f68bae7387c98d95271124a8d7351a6.png)

![825785e0e853328223b7574d159ebbb2.png]( resources/825785e0e853328223b7574d159ebbb2.png)

*You can load the list from:* */usr/share/wordlists/SecLists/Passwords/Common-Credentials/best1050.txt*

*![2df98be33c91e11603108a102e4c468e.png]( resources/2df98be33c91e11603108a102e4c468e.png)*

*Start attack*

*![edc8864aafeebd429a2d12e4cc6c41aa.png]( resources/edc8864aafeebd429a2d12e4cc6c41aa.png)*

*![d16827703e01babf601ace322c5e95bb.png]( resources/d16827703e01babf601ace322c5e95bb.png)*

Check Status tab and 200 is successful code  
![cf4a663d8bdca3ec4f1df088bc53ca0f.png]( resources/cf4a663d8bdca3ec4f1df088bc53ca0f.png)  
\*

Login with admin123

![10114ec6ec275ca267adc6223f952da5.png]( resources/10114ec6ec275ca267adc6223f952da5.png)

![52c03bbd0426fb03ea951272482ce3b3.png]( resources/52c03bbd0426fb03ea951272482ce3b3.png)

pass: admin123

**Answer:** ff4aebffe31b0ffdea9bdd0207a16a3c01ac6c56

**Question #2: Reset Jim’s password!**

Believe it or not, the reset password mechanism can also be exploited! When inputted into the email field in the Forgot Password page, Jim’s security question is set to *“Your eldest siblings middle name?”*.

Looks like his brother’s middle name is **Samuel**

Inputting that into the Forgot Password page allows you to successfully change his password.

You can change it to anything you want!

Click Forgot user password

![fe9353b9e525ce0fc634dcd0cd8948c5.png]( resources/fe9353b9e525ce0fc634dcd0cd8948c5.png)

![bdc0e2fee251b9a8d92c1af13a4d451f.png]( resources/bdc0e2fee251b9a8d92c1af13a4d451f.png)

![b2f74691421d4633c061ed65782f799d.png]( resources/b2f74691421d4633c061ed65782f799d.png)

![bf16bb4d12cabce4dacea1b4a117471b.png]( resources/bf16bb4d12cabce4dacea1b4a117471b.png)

Click Change

![3cfa3027f09bef39f59eb351c93143a4.png]( resources/3cfa3027f09bef39f59eb351c93143a4.png)

security question : Samuel

**Answer**: 3c3e2d6ef99b733b947e92f8e2a9ed08bf57ea63

**Task 5: AH! Don’t look!**

A web application should store and tr Answermit sensitive data safely and securely. But in some cases, the developer may not correctly protect their sensitive data, making it vulnerable.

Most of the time, data protection is not applied consistently across the web application making certain pages accessible to the public. Other times information is leaked to the public without the knowledge of the developer, making the web application vulnerable to an attack.

More information: Sensitive Data Exposure

**Question #1: Access the Confidential Document!**

We can see /ftp/legal.md

We will download the acquisitions.md and save it. It looks like there are other files of interest here as well.

After downloading it, navigate to the home page to receive the flag!

![829e69658c231bc5d045d96b5f8fe1a0.png]( resources/829e69658c231bc5d045d96b5f8fe1a0.png)

![ef0528090f2e0382d472887fc636f766.png]( resources/ef0528090f2e0382d472887fc636f766.png)

On URL write /ftp

![850908fe8b62d1a8f5f7145d62914934.png]( resources/850908fe8b62d1a8f5f7145d62914934.png)

![0506602c304ae0c50d8433428457b531.png]( resources/0506602c304ae0c50d8433428457b531.png)

Download or save page as acquisitions.md

![0ee00f4468760b12ce27f86bd3012257.png]( resources/0ee00f4468760b12ce27f86bd3012257.png)

![5292cad51ff9e33afa6d85ceb75f3fce.png]( resources/5292cad51ff9e33afa6d85ceb75f3fce.png)

**Answer:** 8d2072c6b0a455608ca1a293dc0c9579883fc6a5

**Question #2: Log into MC SafeSearch’s account!**

After watching the video there are certain parts of the song that stand out.

He notes that his password is “Mr. Noodles” but he has replaced some “vowels into zeros”, meaning that he just replaced the o’s into 0's.

We now know the password to the **mc.safesearch@juice-sh.op** account is “**Mr. N00dles**”

![fd77bb7451b38dc7256661d50887c51c.png]( resources/fd77bb7451b38dc7256661d50887c51c.png)

![d9b4d11564a1e6582e3bcccea64a859e.png]( resources/d9b4d11564a1e6582e3bcccea64a859e.png)

**Answer:** bb105418e73708ceccf1a7b2491f434b8f5230e4

**Question #3: Download the Backup file!**

To get around this, we will use a character bypass called “**Poison Null Byte**”. A Poison Null Byte looks like this: ***%00***.

The Poison Null Byte will now look like this: ***%2500***\*.\* Adding this and then a **.md** to the end will Poison Null Byte is actually a **NULL terminator**. By placing a NULL character in the string at a certain byte, the string will tell the server to terminate at that point, nulling the rest of the string.

![6fb083ed569959b7230dd086f6648f31.png]( resources/6fb083ed569959b7230dd086f6648f31.png)

http://10.67.191.103/ftp/package.json.bak%2500.md

![2a1ed41471c49d7853191ba58c154b0a.png]( resources/2a1ed41471c49d7853191ba58c154b0a.png)

![7e96dcd8003d6614b4e119bcec2758fe.png]( resources/7e96dcd8003d6614b4e119bcec2758fe.png)

**Answer:** cfdeea14e8f01b4952722fd0e4a77f1928593c9a

**Task 6: Who’s flying this thing?**

Modern-day systems will allow for multiple users to have access to different pages. Administrators most commonly use an administration page to edit, add and remove different elements of a website. You might use these when you are building a website with programs such as Weebly or Wix.

When Broken Access Control exploits or bugs are found, it will be categorised into one of **two types**:

**Horizontal** Privilege Escalation →  
Occurs when a user can perform an action or access data of another user with the **same** level of permissions.  
**Vertical** Privilege Escalation →  
Occurs when a user can perform an action or access data of another user with a **higher** level of permissions.

**Question #1: Access the administration page!**

We are then going to refresh the page and look for a javascript file for **main-es2015.js**

![4af17e29ba8f6f9bdcc2c0a003e33deb.png]( resources/4af17e29ba8f6f9bdcc2c0a003e33deb.png)

![70770c915ab629a0d218e61110c9f018.png]( resources/70770c915ab629a0d218e61110c9f018.png)

Search Administrator

![d10fd3a78cadd05268328d997ce2d66b.png]( resources/d10fd3a78cadd05268328d997ce2d66b.png)

Logout and login in to Admin account

![17c7b16e697a6de4a0ec29e307f662b7.png]( resources/17c7b16e697a6de4a0ec29e307f662b7.png)

This hints towards a page called “/#/administration” as can be seen by the about path a couple lines below, but going there while not logged in doesn’t work.

As this is an Administrator page, it makes sense that we need to be in the Admin account in order to view it.

A good way to stop users from accessing this is to only load parts of the application that need to be used by them. This stops sensitive information such as an admin page from been leaked or viewed.

change to search tro administration

![87d2d2c82f84352164f95b5b06190a6b.png]( resources/87d2d2c82f84352164f95b5b06190a6b.png)

![ea4f317e0ce399f4a325cba67e23a6f6.png]( resources/ea4f317e0ce399f4a325cba67e23a6f6.png)

![8fec469ea623920ad35b263b15655f33.png]( resources/8fec469ea623920ad35b263b15655f33.png)

**Answer:** 71aeb3b0bf01cc6e488f0207bb62f79b41454a87

**Question #2: View another user’s shopping basket!**

Login to the Admin account and click on ‘Your Basket’. Make sure Burp is running so you can capture the request!

![f093b6b57882c940783f15ad08eb34e8.png]( resources/f093b6b57882c940783f15ad08eb34e8.png)

Make sure intercept is on and Burp enable  
![191ee4abde411a5759ba8cb1517edd0f.png]( resources/191ee4abde411a5759ba8cb1517edd0f.png)

click basket  
![3d43ec092eec4c60395613a3995b8eca.png]( resources/3d43ec092eec4c60395613a3995b8eca.png)

![250e7efee2774d036e0ea984b6403c8b.png]( resources/250e7efee2774d036e0ea984b6403c8b.png)

Forward each request until you see: *GET /rest/basket/1 HTTP/1.1*

Now, we are going to change the number 1 after /basket/ to 2

![a1a86272130a8c09399f461910397ecd.png]( resources/a1a86272130a8c09399f461910397ecd.png)

Click Forward

![e13d49f8f52ef0c451e55e6e274a21d4.png]( resources/e13d49f8f52ef0c451e55e6e274a21d4.png)

**Answer:** e6982b34b6734ceadd28e5019b251f929a80b815

**Question #3: Remove all 5-star reviews!**

Navigate to the http://machine_ip/#/administration page again and click the **bin icon** next to the review with 5 stars!

Disable to Burp and Intercept

![4389510080111c64641a50017552e022.png]( resources/4389510080111c64641a50017552e022.png)

Cnage the administration

![4ea2f9ba883156d77275abbc2dec0c8c.png]( resources/4ea2f9ba883156d77275abbc2dec0c8c.png)

![e8b9655e2c2c66ca5461974375347081.png]( resources/e8b9655e2c2c66ca5461974375347081.png)

![443ba6eb5642b2d93001acecd85c6f39.png]( resources/443ba6eb5642b2d93001acecd85c6f39.png)

**Answer:** 78231b75c0b2180b7e964dcbb1ab3c3f58639f2e

**Task 7: Where did that come from?**

XSS or Cross-site scripting is a vulnerability that allows attackers to run javascript in web applications. These are one of the most found bugs in web applications. Their complexity ranges from easy to extremely hard, as each web application parses the queries in a different way.

**There are three major types of XSS attacks:**

DOM (Special)  
**DOM XSS** *(Document Object Model-based Cross-site Scripting)* uses the HTML environment to execute malicious javascript. This type of attack commonly uses the *&lt;script&gt;&lt;/script&gt;* HTML tag.  
Persistent (Server-side)  
**Persistent XSS** is javascript that is run when the server loads the page containing it. These can occur when the server does not sanitise the user data when it is **uploaded** to a page. These are commonly found on blog posts.  
Reflected (Client-side)  
**Reflected XSS** is javascript that is run on the client-side end of the web application. These are most commonly found when the server doesn’t sanitise **search** data.

**Question #1: Perform a DOM XSS!**

We will be using the iframe element with a javascript alert tag:

*&lt;iframe src=”javascript:alert(\`xss\`)”&gt;*

Inputting this into the **search bar** will trigger the alert.

![ffb65714b572bd73ce3a35601558d0e6.png]( resources/ffb65714b572bd73ce3a35601558d0e6.png)

![7cb606dbda6f50feead085302eae4c8b.png]( resources/7cb606dbda6f50feead085302eae4c8b.png)

**Answer:** 4a31a4fe0954199566e360a873802bf64d0d0a84

**#2 Question #2: Perform a persistent XSS!**

First, login to the **admin** account.

We are going to navigate to the “**Last Login IP**” page for this attack.

Make sure that Burp **intercept is on**, so it will catch the logout request.

![0a24ba41cfe609293ebea820ddbcfdda.png]( resources/0a24ba41cfe609293ebea820ddbcfdda.png)

![4775bd028e823641fbfd71dc24ab5717.png]( resources/4775bd028e823641fbfd71dc24ab5717.png)

Enabled the Burp  
**![5407890edfd0e09b856e6bcdbc3c98c6.png]( resources/5407890edfd0e09b856e6bcdbc3c98c6.png)**

Logout he admin account  
![9d83daf91093746c0c4962d856331458.png]( resources/9d83daf91093746c0c4962d856331458.png)

Add the header

![c0be14a345356bba044af5bf123c56ac.png]( resources/c0be14a345356bba044af5bf123c56ac.png)

We will then head over to the Headers tab where we will add a new header:

Click forward

![7b9dd27c01ec2ee0b4bb0bdda24c3212.png]( resources/7b9dd27c01ec2ee0b4bb0bdda24c3212.png)

**Answer:** c37da14686b69a220fd9febd09bb9593e7d0539f

**Question #3: Perform a reflected XSS!**

Disabl;ed the Burp\[ and OIntercept  
![8e158c691ff4ee77b796c57be18df3cd.png]( resources/8e158c691ff4ee77b796c57be18df3cd.png)

First, we are going to need to be on the right page to perform the reflected XSS!

**Login** into the **admin account** and navigate to the ‘**Order History**

**![77e302b781518ee919a126fdf7051f72.png]( resources/77e302b781518ee919a126fdf7051f72.png)**

From there you will see a “**Truck**” icon, clicking on that will bring you to the track result page. You will also see that there is an id paired with the order.

![729d6d0f4a48efbb67246823832315af.png]( resources/729d6d0f4a48efbb67246823832315af.png)

![8fc618714de330c06a292a34e8271c39.png]( resources/8fc618714de330c06a292a34e8271c39.png)

![6d0c7d78d3be092f460cc5731503e7a9.png]( resources/6d0c7d78d3be092f460cc5731503e7a9.png)

![c1ebc93144555d837767c535ef5cd7ec.png]( resources/c1ebc93144555d837767c535ef5cd7ec.png)

![c16ff45ab86d852f5740b4ad725ce95a.png]( resources/c16ff45ab86d852f5740b4ad725ce95a.png)

Update the url

![5b36a1fbae224af7a518e40127709c0b.png]( resources/5b36a1fbae224af7a518e40127709c0b.png)

![47a1e61d783679ed24e8c97184371f8e.png]( resources/47a1e61d783679ed24e8c97184371f8e.png)

**Answer:** 305021787d3e9cd9cebc057a021c2504550bb3b6

**Task 8: Exploration!**

If you wish to tackle some of the **harder** challenges that were not covered within this room, check out the **/#/score-board/** section on Juice-shop. Here you can see your completed tasks as well as other tasks in varying difficulty.

**Access the → /#/score-board/ page**

![f318c9e1ace1cb15c2ef43aef1604528.png]( resources/f318c9e1ace1cb15c2ef43aef1604528.png)

**Answer:** 2614339936e8282e2f820f023d4d998a1f95e02a
