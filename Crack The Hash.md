THM Link: https://tryhackme.com/r/room/crackthehash

**Level 1**

First try to find hash type with hash-identifier command

![7a7e6563088bdb551e40b4192d4536f5.png](resources/7a7e6563088bdb551e40b4192d4536f5.png)

Once we identify hash type then we can find hash mode

https://hashcat.net/wiki/doku.php?id=example_hashes

![d0e15022610f11c3f5a20a951b170d54.png](../../../../_resources/d0e15022610f11c3f5a20a951b170d54.png)

We have hash type and hash mode -- > we can write our commend to find hash password

hashcat -m 0 48bb6e862e54f2a795ffc4e541caed4d /root/Desktop/wordlists/rockyou.txt

\`hashcat -m\` refers to the "hash mode" in \*\*Hashcat\*\*. The \`-m\` option specifies the hash type that you're trying to crack.

Different algorithms and formats use different hash types, so selecting the correct one is crucial for effective cracking.

Each \`-m\` value corresponds to a specific hash algorithm or encoding format. For example:

\- \`-m 0\`: MD5  
\- \`-m 100\`: SHA-1  
\- \`-m 1400\`: SHA-256  
\- \`-m 1800\`: SHA-512  
\- \`-m 2500\`: WPA/WPA2 (handshake)  
\- \`-m 3200\`: bcrypt  
\- \`-m 5600\`: NetNTLMv2  
\- \`-m 1000\`: NTLM (Windows hashes)

![b8cf2b6b1f7f6a5b4250b2a9d023e7c3.png](../../../../_resources/b8cf2b6b1f7f6a5b4250b2a9d023e7c3.png)

![a1807ac0310a14814ed7cbc9bb001924.png](../../../../_resources/a1807ac0310a14814ed7cbc9bb001924.png)

Can you complete the level 1 tasks by cracking the hashes?

**Q:** 48bb6e862e54f2a795ffc4e541caed4d -- easy

The hint says this is an md5 hash. Command to crack the hash: hashcat -m 0 hash1_1.txt /usr/share/wordlists/rockyou.txt --show  
![b8cf2b6b1f7f6a5b4250b2a9d023e7c3.png](../../../../_resources/b8cf2b6b1f7f6a5b4250b2a9d023e7c3.png)

![a1807ac0310a14814ed7cbc9bb001924.png](../../../../_resources/a1807ac0310a14814ed7cbc9bb001924.png)

**Q:** CBFDAC6008F9CAB4083784CBD1874F76618D2A97  -- password123

![809bf6df2d337874dcc21da85aa3d2e6.png](../../../../_resources/809bf6df2d337874dcc21da85aa3d2e6.png)

The hint says this is a sha.. hash. Since hashcat can crack only 4 types of sha hashes, I tried all of them until one was successful. Turns out it was a SHA1 hash. Command to crack the hash :

hashcat -m 100 CBFDAC6008F9CAB4083784CBD1874F76618D2A97 /usr/share/wordlists/rockyou.txt

![e1554c74c23ad9c9929a01ccc3d2630d.png](../../../../_resources/e1554c74c23ad9c9929a01ccc3d2630d.png)  
![b6cd94706362b8cde50e5ca86bfe01ab.png](../../../../_resources/b6cd94706362b8cde50e5ca86bfe01ab.png)

**Q:** 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032 -- letmein

![802eeb7960ea1c057e4747e2660da76b.png](../../../../_resources/802eeb7960ea1c057e4747e2660da76b.png)

The hint says this is a sha.. hash. Same strategy used in the previous question. Turns out it was a SHA256 hash. Command used to crack the hash:  
hashcat -m 1400 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032 /usr/share/wordlists/rockyou.txt  
![b0cc5e724abf0609405987e94e265fbf.png](../../../../_resources/b0cc5e724abf0609405987e94e265fbf.png)  
![c6ebb055c38e287824eb0e2eade212cb.png](../../../../_resources/c6ebb055c38e287824eb0e2eade212cb.png)

**Q:** \$2y121212Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom -- bleh

https://www.tunnelsup.com/hash-analyzer/  
![471d330f737cb007a4574588c8d18ba2.png](../../../../_resources/471d330f737cb007a4574588c8d18ba2.png)

https://hashes.com/en/decrypt/hash  
![340e37d567a06204805493ab029487a8.png](../../../../_resources/340e37d567a06204805493ab029487a8.png)

![de68119547d16c142821861e7cab3a99.png](../../../../_resources/de68119547d16c142821861e7cab3a99.png)

The hint says this is a bcrypt hash. Command used to crack the hash: hashcat -m 3200 hash1_4.txt /usr/share/wordlists/rockyou.txt

**Q:** 279412f945939ba78ce0758d3fd83daa -- Eternity22

![0e8a1d9a39330b86a147672fd2c27399.png](../../../../_resources/0e8a1d9a39330b86a147672fd2c27399.png)

The hint says this is a md4 hash. I tried using the following command used to crack the hash: hashcat -m 900 hash1_5.txt /usr/share/wordlists/rockyou.txt, yet hashcat didn't find the password in the rockyou.txt file, so I used the following online tool to crack the hash: link.  
https://crackstation.net/  
![a63dc036fff1be18a221af5434542e53.png](../../../../_resources/a63dc036fff1be18a221af5434542e53.png)

**Task 2: Level 2**  
This task increases the difficulty. All of the answers will be in the classic rock you password list.

You might have to start using hashcat here and not online tools. It might also be handy to look at some example hashes on hashcats page.

Hash: F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85

![bf4425e11a8a2dd3fdc1cd8d8e0a9a81.png](../../../../_resources/bf4425e11a8a2dd3fdc1cd8d8e0a9a81.png)

![916bef966dd4bfe1210c2f8a78f97b12.png](../../../../_resources/916bef966dd4bfe1210c2f8a78f97b12.png)

![df2a9fd33705817dfc89ab6da9d42da0.png](../../../../_resources/df2a9fd33705817dfc89ab6da9d42da0.png)

![c016d893b3a95753e83ed384ba0fc087.png](../../../../_resources/c016d893b3a95753e83ed384ba0fc087.png)

It seems this is a SHA256 hash. Command used to crack the hash: hashcat -m 1400 task_1.txt /usr/share/wordlists/rockyou.txt

**paule**

**Hash: 1DFECA0C002AE40B8619ECF94819CC1B -- n63umy8lkf4i**  
The hint says this is a NTLM hash. Command used to crack the hash: hashcat -m 1000 hash2_2.txt /usr/share/wordlists/rockyou.txt

![34701e1d89c509858d9338527f182cfa.png](../../../../_resources/34701e1d89c509858d9338527f182cfa.png)

![3e646278d16b952cdb4d086fd07b4cd6.png](../../../../_resources/3e646278d16b952cdb4d086fd07b4cd6.png)  
![4b16547170f50b3a501e42088f877902.png](../../../../_resources/4b16547170f50b3a501e42088f877902.png)

n63umy8lkf4i  
![96bce14bdb09992988926b5a9c2faceb.png](../../../../_resources/96bce14bdb09992988926b5a9c2faceb.png)

Hash: 666aReallyHardSalt\$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.

Salt: aReallyHardSalt

![98dc8e8c99255bb5994b61e56dcc8e95.png](../../../../_resources/98dc8e8c99255bb5994b61e56dcc8e95.png)  
![66fdafdea9cae43064b9bea1c929f235.png](../../../../_resources/66fdafdea9cae43064b9bea1c929f235.png)

This one seems to be a SHA-512(Unix) hash. Command used to crack the hash: hashcat -m 1800 hash2_3.txt /usr/share/wordlists/rockyou.txt

waka99

Hash: e5d8870e5bdd26602cab8dbe07a942c8669e56d6  
Salt: tryhackme

hashcat -m 160 task_3.txt /root/Desktop/wordlists  
hashcat (v6.2.6) starting

![b87a60e643a7e1ef20418c17ff0e53bb.png](../../../../_resources/b87a60e643a7e1ef20418c17ff0e53bb.png)  
![e384faefd33a0f4925365ea4d7a24c68.png](../../../../_resources/e384faefd33a0f4925365ea4d7a24c68.png)  
the above image we did not put salt  
![74650203db158345d92500754ede2002.png](../../../../_resources/74650203db158345d92500754ede2002.png)  
![b6cde8571a13bf618aa1b32dbdec1ccf.png](../../../../_resources/b6cde8571a13bf618aa1b32dbdec1ccf.png)

The hint says this one is a HMAC-SHA1 hash. Command used to crack the hash: hashcat -m 160 hash2_4.txt /usr/share/wordlists/rockyou.txt.

481616481616
