---
published: false
---
Hello guys, for the seonc time in a row, I've had the honor of writing the DF challenges for the final round of ASCWG 2021, I've created 6 challenges for this round, 2 of them were warmup challenges, while the other 4 were DF challenges. I hope the players have nejoyed solving these challenges. 

<p> 1st challenge: ```I got some ***ngs attached.```</p>
  
  ![strings_attached.png]({{site.baseurl}}/assets/strings_attached.png)
 
  The challenge is extremely easy and aims at teaching the players to learn the basic usage of ```strings``` utility in linux. 
  
  ![strings.png]({{site.baseurl}}/assets/strings.png)

  and we get the flag.
  
![Flag_strings.png]({{site.baseurl}}/assets/Flag_strings.png)

  Flag: ```ASCWG{Str1ngsS_1s_n1C3}```
  
  <p> 2nd challenge: ```Cats are listening``` </p>
  
  ![]({{site.baseurl}}/assets/cats%20are%20listening.png)
  ![cats are listening.png]({{site.baseurl}}/assets/cats are listening.png)

Description: ```Now let's try to get the flag from the pcap file.```

The challenge is relatively easy, we simply need to extract the sent file, to do so we need to check the conversations in this pcap file. We do so by opening the file in wireshark.

![]({{site.baseurl}}/assets/2021-09-09%2000_26_05-Window.png)![2021-09-09 00_26_05-Window.png]({{site.baseurl}}/assets/2021-09-09 00_26_05-Window.png)



Then, we notice a conversation between the destination's local IP and another local IP at the source. This conversation is the one we're looking for.

Then we follow TCP stream on that conversation, and we are able to see a base64 encoded string.
![]({{site.baseurl}}/assets/2021-09-09%2000_31_02-Window.png)
![2021-09-09 00_31_02-Window.png]({{site.baseurl}}/assets/2021-09-09 00_31_02-Window.png)

Then we copy the contents to [cyberchef](https://gchq.github.io/CyberChef), and we decode the string to get a PNG file. 
![]({{site.baseurl}}/assets/2021-09-09%2000_33_31-Window.png)![2021-09-09 00_33_31-Window.png]({{site.baseurl}}/assets/2021-09-09 00_33_31-Window.png)

Downloading the file, we get the flag.

![]({{site.baseurl}}/assets/2021-09-09%2000_34_51-Window.png)![2021-09-09 00_34_51-Window.png]({{site.baseurl}}/assets/2021-09-09 00_34_51-Window.png)

Yeah I added some memes to the challenges :"D.

Flag: ```ASCWG{Str1ngsS_1s_n1C3}```

<p> 3rd challenge: Archive Lost</p>

![]({{site.baseurl}}/assets/Archive%20Lost.png)
![Archive Lost.png]({{site.baseurl}}/assets/Archive Lost.png)

Description: ```One of our employees arvhiced some important documents, and he damaged the file in the process. Can you fix the archive to get the file?```

The challenge aims to teach the players about rar file structure and that there are some other magic numbers beyond the first bytes that matter as much as them.

Attempting to open the rar file, we get a prompt that says it's corrupt, we try to look at the file in a hex editor, and we notice something weird. 
![weird.png]({{site.baseurl}}/assets/weird.png)

Now, we need to fix the magic numbers. Going to that [link](https://docs.fileformat.com/compression/rar/)
We find that the 52 digit needs to be changed to be 01, doing so will enable us to open the rar archive but we need a password. The password is ```mollycat```, the password number 40001 in rockyou.txt.

Opening the rar archive with the password, we get the flag.

Flag: ```ASCWG{1337_r@R_F0r1ns3c$_An@ly$t}```

<p> 4th challenge: ```I'm not broken, you are.``` </p>

![broken.png]({{site.baseurl}}/assets/broken.png)

Description: ```During an IR investigation our junior IR engineer screwed up the filesystem, which has the evidence to imprison the notorious cyber criminal Adel Shakal. Can you fix the img and retrieve the file?```

This challenge aims to teach the players about linux filesystem structure forensics and how to repair them.

First of this challenge could be solved in two different methods, the first of which is the intended method, which I will explain, and the second one was discovered by the king of the unintended methods :"D [Mohammed Hassan](https://0xmohammed.github.io) so big shout out to him for discovering it, and I will explaing it below.

We simply need to fix the primary superblock to make the filesystem readable.

![]({{site.baseurl}}/assets/bad_superblock.png)![bad_superblock.png]({{site.baseurl}}/assets/bad_superblock.png)

Now, we need to fix the disk, so we need to execute this command first ```sudo dd if=sdb1.img of=/dev/sdb1 bs=1k conv=noerror```

![repair-I.png]({{site.baseurl}}/assets/repair-I.png)

Then we need to execute another command ```sudo fsck /dev/sdb1```
![repair-II.png]({{site.baseurl}}/assets/repair-II.png)

Then we can mount the disk normally, and view its contents. 
![fixed.png]({{site.baseurl}}/_posts/fixed.png)

Flag: ```ASCWG{B@ckup_Sup3rB10ck$_@r3_l!f3_Sav3r$```

I'm a solo leveling fan :"D

Second method: 

Just open the disk img file in R-Studio program and it will repair it by itself :"D
 
<p> 5th Challenge: ```Lost in PDF```</p>
  
  ![Lost.png]({{site.baseurl}}/_posts/Lost.png)
  
  Description: ```A hacker managed to send a malicious PDF file to NullzCorp's CEO, the file was executed and the hacker was able to get into the machine. Can you recover the IP of the hacker's machine?```
  
  Okay so we have a malicious PDF file, which has the flag. We need to see the injected code into the PDF file, to do so we can use multiple tools such as ```pdf-parser```, ```ppdf``` but none of them will be useful beyond that. The tool we need to use it is called [PDF stream dumper](http://sandsprite.com/blogs/index.php?uid=7&pid=57)
  
  Downloaing the tool, and opening the file in it we will notice something interesting.
  
  ![random.png]({{site.baseurl}}/assets/random.png)

The JS code injected into the PDF is obfuscated, at first glance it appears to be base64; but it's not as the obfuscation is random
