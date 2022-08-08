---
published: true
---
							ASCWG 2022 Qual Round Forensics Challenges Writeup

Hello guys,

For the third year in a row I was honored to write the forensics challenges for the Qualification round of ASCWG 2022. I created 6![Weird Fs .png]({{site.baseurl}}/assets/Weird Fs .png)
 challenges for this year (2 of them could be solved through following the same steps but with some slight variation). I hope the participants have enjoyed solving these challenges.

1st Challenge: ```warmup1```
![]({{site.baseurl}}/assets/Warmup%201%20.png)
![Warmup 1 .png]({{site.baseurl}}/assets/Warmup 1 .png)

The challenge is extremely easy, we just need to ```cat``` the file. 
![Warmup1-Solution.png]({{site.baseurl}}/assets/Warmup1-Solution.png)

From the description we're looking for the registry key which stores the Wireless networks' IPs our devices connect to. After getting the value, we need to get the sha1sum of the IP we found.

Flag:```ASCWG{1d6ab599105a065ccdad99759d5e7509e7502aff}```

![Warmup1-2S.png]({{site.baseurl}}/assets/Warmup1-2S.png)

2nd Challenge: ```Warmup2```

![]({{site.baseurl}}/assets/Warmup%202%20.png)
![Warmup 2 .png]({{site.baseurl}}/assets/Warmup 2 .png)

In this challenge we need to get the payload from a picture.

Let's our old faithful tool ```strings```
![Warmup2-S1.png]({{site.baseurl}}/assets/Warmup2-S1.png)

We find out it's URL encoded payload, decoding on ```cyberchef```, we get the following.
![2022-08-08 20_52_30-URL Decode - CyberChef — Mozilla Firefox.png]({{site.baseurl}}/assets/2022-08-08 20_52_30-URL Decode - CyberChef — Mozilla Firefox.png)

The flag is the sha1sum of the payloads' IP

Flag:```ASCWG{f778029a0c2cecd23e90986ca394952a5a770e8e}```

3rd Challenge: ```Weird FS```

![]({{site.baseurl}}/assets/Weird%20Fs%20.png)

This challenge was aiming to teach the participants about APFS which isn't mentioned that much in CTFs, so I wanted some light and easy challenge about it.

To mount the disk image, we can use various tools such as: FTK imager, UFS explorer, paragon for APFS, and the list goes on.

In my case I used UFS explorer just for variety.

![2022-08-08 20_59_31-UFS Explorer Standard Access - version 5.31.png]({{site.baseurl}}/assets/2022-08-08 20_59_31-UFS Explorer Standard Access - version 5.31.png)

We then extract the ```flag.zip``` file, of course the archive is password protected. The password is ```juelma```, which can be found in ```rockyou.txt```.
![2022-08-08 21_02_28-Flag.txt - Notepad.png]({{site.baseurl}}/assets/2022-08-08 21_02_28-Flag.txt - Notepad.png)

Flag: ```ASCWG{M4C_4N6_1$_Co0l}```

4th Challenge: ```Persistent Ghost```

![Persistent Ghost.png]({{site.baseurl}}/assets/Persistent Ghost.png)


This challenge was aiming to teach participants about persistence in registry.

The keys in question were: 

1.```HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run```
2.```HKEY_USERS\SOFTWARE\Microsoft\Windows\CurrentVersion\Run```
3.```HKEY_USERS\SOFTWARE\Microsoft\Windows\CurrentVersion\Screensavers\ssText3d\Screen 3```
There was a rabbit hole located in the bootexecute key, the rabbit hole was this picture :"D
![medium-challenge.png]({{site.baseurl}}/assets/medium-challenge.png)

Going for the registry keys mentioned above we are able to collect 3 pieces of B64 encoded string, they are located in part-I
![Persistent Ghost-S1.png]({{site.baseurl}}/assets/Persistent Ghost-S1.png)

Collecting the 3 parts from the 3 keys, we get the following B64 encoded code.
![Persistent Ghost-S2.png]({{site.baseurl}}/assets/Persistent Ghost-S2.png)

Flag: ```ASCWG{P3rs1stenc3_1s1337_Hackerz_sk1llz}```

5th Challenge: ``` Walk Down Memory Lane```

![Walk Down Memory Lane.png]({{site.baseurl}}/assets/Walk Down Memory Lane.png)

In this challenge I was aiming to make participants read and learn about windbg from memory analysis prospective. 

To solve this challenge we need to set up our environment, we need to import the proper symbols to interpret the memory dump we're about to analyze. 

![windbg-1.png]({{site.baseurl}}/_assets/windbg-1.png)
![windbg-2.png]({{site.baseurl}}/assets/windbg-2.png)

We import the symbols from microsoft.

We then import the .DMP file as a crash dump, and let's start analyzing. 

![windbg-3.png]({{site.baseurl}}/assets/windbg-3.png)

We click or type on the command in front of us ```!analyze -v``` and let the debugger starts analyzing the dump, after it finishes, we type the following command which will display a formatted view of the information in the process environment block.

![windbg-4.png]({{site.baseurl}}/assets/windbg-4.png)

After looking at the DLLs, we need to list the modules used by the process we type command ``` lm f```

![windbg-5.png]({{site.baseurl}}/assets/windbg-5.png)

the module we're looking for is the ```iertutil.dll```

Clicking on its hyperlink, we get the memory address inwhich it stores its data.

![windbg-6.png]({{site.baseurl}}/assets/windbg-6.png)

Perfect, now we have the address let's search this dump for anything related to the flag in question, since the flag format start with ASCWG, then we can look for it.
![windbg-7.png]({{site.baseurl}}/assets/windbg-7.png)

Now let's dump the memory contents resided in this address, to do so accurately we need either to go forward or backward a little bit, after some going back and forth, we can get the full data by dumping the memory from the addresses mentioned below. 

![windbg-8.png]({{site.baseurl}}/assets/windbg-8.png)

Flag:```ASCWG{W1ndbG_!$_Sup3R_C0o1}```

6th Challenge:```Walk down memory lane 2```

We follow the same steps as the previous challenge, except for the following:

![windbg-10.png]({{site.baseurl}}/assets/windbg-10.png)
![]({{site.baseurl}}/assets/windbg-11.png)


And this concludes the challenges I created for ASCWG Qual round, I hope the players enjoyed solving them and learned some new things or techniques.
