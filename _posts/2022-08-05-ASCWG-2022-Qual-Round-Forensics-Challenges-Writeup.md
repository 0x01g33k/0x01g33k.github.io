---
published: false
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

