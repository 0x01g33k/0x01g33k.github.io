---
published: true
---
Hello guys, for the second time in a row, I've had the honor of writing the DF challenges for the final round of ASCWG 2021, I've created 6 challenges for this round, 2 of them were warmup challenges, while the other 4 were DF challenges. I hope the players have enjoyed solving these challenges. 

1st challenge: ```I got some ***ngs attached``` 
  
  ![strings_attached.png]({{site.baseurl}}/assets/strings_attached.png)
 
  The challenge is extremely easy and aims at teaching the players to learn the basic usage of ```strings``` utility in linux. 
  
  ![strings.png]({{site.baseurl}}/assets/strings.png)

  and we get the flag.
  
![Flag_strings.png]({{site.baseurl}}/assets/Flag_strings.png)

  Flag: ```ASCWG{Str1ngsS_1s_n1C3}```
  
  
  
  2nd challenge: ```Cats are listening``` 
  
  ![cats are listening.png]({{site.baseurl}}/assets/cats are listening.png)

Description: ```Now let's try to get the flag from the pcap file.```

The challenge is relatively easy, we simply need to extract the sent file, to do so we need to check the conversations in this pcap file. We do so by opening the file in wireshark.

![2021-09-09 00_26_05-Window.png]({{site.baseurl}}/assets/2021-09-09 00_26_05-Window.png)


Then, we notice a conversation between the destination's local IP and another local IP at the source. This conversation is the one we're looking for.

Then we follow TCP stream on that conversation, and we are able to see a base64 encoded string.

![2021-09-09 00_31_02-Window.png]({{site.baseurl}}/assets/2021-09-09 00_31_02-Window.png)

Then we copy the contents to [cyberchef](https://gchq.github.io/CyberChef), and we decode the string to get a PNG file. 

![2021-09-09 00_33_31-Window.png]({{site.baseurl}}/assets/2021-09-09 00_33_31-Window.png)

Downloading the file, we get the flag.

![2021-09-09 00_34_51-Window.png]({{site.baseurl}}/assets/2021-09-09 00_34_51-Window.png)

Yeah I added some memes to the challenges :"D.

Flag: ```ASCWG{Netw0rk_F0r1ns3cs_1s_N!c3}```



3rd challenge: ```Archive Lost```

![]({{site.baseurl}}/assets/Archive%20Lost.png)


Description: ```One of our employees archived some important documents, and he damaged the file in the process. Can you fix the archive to get the file?```

The challenge aims to teach the players about rar file structure and that there are some other magic numbers beyond the first bytes that matter as much as them.

Attempting to open the rar file, we get a prompt that says it's corrupt, we try to look at the file in a hex editor, and we notice something weird. 
![weird.png]({{site.baseurl}}/assets/weird.png)

Now, we need to fix the magic numbers. Going to that [link](https://docs.fileformat.com/compression/rar/)
We find that the 52 digit needs to be changed to be 01, doing so will enable us to open the rar archive but we need a password. The password is ```mollycat```, the password number 40001 in rockyou.txt.

Opening the rar archive with the password, we get the flag.

Flag: ```ASCWG{1337_r@R_F0r1ns3c$_An@ly$t}```



4th challenge: ```I'm not broken, you are.``` 

![broken.png]({{site.baseurl}}/assets/broken.png)

Description: ```During an IR investigation our junior IR engineer screwed up the filesystem, which has the evidence to imprison the notorious cyber criminal Adel Shakal. Can you fix the img and retrieve the file?```

This challenge aims to teach the players about linux filesystem structure forensics and how to repair them.

First of this challenge could be solved in two different methods, the first of which is the intended method, which I will explain, and the second one was discovered by the king of the unintended methods :"D [Mohammed Hassan](https://0xmohammed.github.io) so big shout out to him for discovering it, and I will explain it below.

We simply need to fix the primary superblock to make the filesystem readable.

![bad_superblock.png]({{site.baseurl}}/assets/bad_superblock.png)

Now, we need to fix the disk, so we need to execute this command first ```sudo dd if=sdb1.img of=/dev/sdb1 bs=1k conv=noerror```

![repair-I.png]({{site.baseurl}}/assets/repair-I.png)

Then we need to execute another command ```sudo fsck /dev/sdb1```
![repair-II.png]({{site.baseurl}}/assets/repair-II.png)

Then we can mount the disk normally, and view its contents. 
![fixed.png]({{site.baseurl}}/assets/fixed.png)


Flag: ```ASCWG{B@ckup_Sup3rB10ck$_@r3_l!f3_Sav3r$```

I'm a solo leveling fan :"D

Second method: 

Just open the disk img file in R-Studio program and it will repair it by itself :"D


 
5th Challenge: ```Lost in PDF```
  
  ![Lost.png]({{site.baseurl}}/assets/Lost.png)
  
  Description: ```A hacker managed to send a malicious PDF file to NullzCorp's CEO, the file was executed and the hacker was able to get into the machine. Can you recover the IP of the hacker's machine?```
  
  Okay so we have a malicious PDF file, which has the flag. We need to see the injected code into the PDF file, to do so we can use multiple tools such as ```pdf-parser```, ```ppdf``` but none of them will be useful beyond that. The tool we need to use it is called [PDF stream dumper](http://sandsprite.com/blogs/index.php?uid=7&pid=57)
  
  Downloaing the tool, and opening the file in it we will notice something interesting.
  
  ![random.png]({{site.baseurl}}/assets/random.png)

The JS code injected into the PDF is obfuscated, at first glance it appears to be base64; but it's not as the obfuscation is random. So we need to analyze the shellcode.

From the tabs above, we select ```Javascript_UI```, and a new prompt will appear. 

![prompt.png]({{site.baseurl}}/assets/prompt.png)

Now we select the shellcode from within the double quotes for analysis, and from the tabs above we select ```shellcode_analysis``` , and from the drop down menu, we select ```sclog```.

A new window will appear, we need only to launch the analyzer, a cmd will popup with the IP and port.

![analyze.png]({{site.baseurl}}/assets/analyze.png)


![prompt-II.png]({{site.baseurl}}/assets/prompt-II.png)

malicious IP: 192.168.197.128

Flag: ```ASCWG{d5c61062aa49378381317b41f097787b1df32508}```



6th Challenge: ```apRoot```

![2021-09-07 12_28_47-Window.png]({{site.baseurl}}/assets/2021-09-07 12_28_47-Window.png)

Description: ```The villianous APT group known as Dark_Lu1z have injected the CEO of elnaho company phone with their malicious apk file, the company's IR team wasn't able to respond properly to the incident, as the system logs were encrypted with a weak symmetric encryption scheme and the key was hidden in the certificate of the apk. Can you recover the PID and the package name?```

```NOTE: flag format is ASCWG{SHA1sum} the SHA1sum is the hash of the PID:PackageName```

Okay, so this challenge was aimed to get a new diretion into android forensics. 

We have an apk file, which is signed, and there's a hidden key in it that encrypted the system logs. 

We need to extract the certificate first, so we change the apk extension to zip.

![unzip.png]({{site.baseurl}}/assets/unzip.png)


Inside the META/INF directory, we get the certificate which its name is HACKED.RSA

![rsa.png]({{site.baseurl}}/assets/rsa.png)

Now the real thing begins, we need to extract the certificate details, so we will use this command ```openssl pkcs7 -inform DER -in HACKED.RSA -noout -print_certs -text```

![rsa-II.png]({{site.baseurl}}/assets/rsa-II.png)

![rsa-iii.png]({{site.baseurl}}/assets/rsa-iii.png)

In order to decrypt the file we need to split the X509v3 key identifier, The encryption scheme used to encrypt the logs was RC2. The key was: ```53:63:8E:5D:75:79:3C:17:0B:07:C4```, and the IV: ```:11:2C:A5:4A:F7:25:B6:43:15```

Now, we go to cyberchef and decrypt the file. 
![decrypt.png]({{site.baseurl}}/assets/decrypt.png)

We download the file, and we look it up for the package name, which can be found in ```AndroidManifest.xml``` file. The package name is: ```com.metasploit.stage```

![Hash.png]({{site.baseurl}}/assets/Hash.png)

Hashing the PID number which is ```1973``` with the package name we get the flag.
![hash-2.png]({{site.baseurl}}/assets/hash-2.png)

Flag: ```ASCWG{4263bb45bee81173edc3bdd9f03813648ca60036}```

And this concludes the challenges for the final round of ASCWG 2021, I hope the players have learned something new from solving these challenges and hopefully see you again next year.
