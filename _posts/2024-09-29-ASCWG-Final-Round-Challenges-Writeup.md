
Hello guys, for  the 5th year in a row I've had the honor to write the challenges for Arab Security Cyber WarGames (ASCWG). This is the writeup for the final round's challenges for this year's edition. Without any further adue, let's jump right in.

## 1st Challenge: Validate what you wrote

![validate.png]({{site.baseurl}}/assets/validate.png)

Description:```During an investigation, one of our DF investigators stumbled upon a file which the suspect claims is a forged, we had the original file, but we need the hash of the file which the investigator found. Could you get the file's hash?```

```NOTE:hash is in an uppercase format```

TL;DR: Okay, so in this challenge I wanted to teach the players about a new artifact related to ```Windows 11```, as of Win11, we don't need to save any file we open in notepad since it stores all of the file's content and data in a ```bin file```, which is stored on the drive itself, the file's content is loaded each and everytime we open notepad, the bin file has a lot of data regarding each and every file we open in notepad, one of the important artifacts is the hash value for the opened files, and that was the flag in this challenge. You can understand the internals of the artifact perfectly in this eloquent blogpost https://u0041.co/posts/articals/exploring-windows-artifacts-notepad-files/ made by [AbdulRhman Alfaifi](https://twitter.com/A__Alfaifi)

I wanted the players to extract the hash of the file which I opened in notepad. So, first thing people usually do is run ```cat``` on the file. Upon doing so we find the following:

![2024-09-30 13_51_42-Kali Linux - VMware Workstation.png]({{site.baseurl}}/assets/2024-09-30 13_51_42-Kali Linux - VMware Workstation.png)

It's a B64 encoded strings, if we decoded the strings in CyberChef we find it's a picture file, which is a meme that I made for the challenge :D
![2024-09-30 14_00_25-Kali Linux - VMware Workstation.png]({{site.baseurl}}/assets/2024-09-30 14_00_25-Kali Linux - VMware Workstation.png)

Okay, so now we need to dig deeper, we open the ```bin file``` in a hex editor, and if we follow the behavior of the artifact we will find some weird values before the first bytes of the B64 encoded string.
![2024-09-30 14_03_06-Free Hex Editor Neo.png]({{site.baseurl}}/assets/2024-09-30 14_03_06-Free Hex Editor Neo.png)

Okay, so now we need to get the hash of the file, the hash starts at ```ed 17 f6```, following these values to get the 64 bytes of the SHA2 hash, we reach value ```c1 0b```, we convert the hash to uppercase and we get the flag. 

```ASCWG{ED17F698FDD16C6802DB13BE54B142EDAA9030584C370CDD093A2884FF7DC10B}```

## 2nd Challenge: Did you check our new Policy?

![policy.png]({{site.baseurl}}/assets/policy.png)

Description:```an attacker managed to alter the newly pushed policies for surfing the web for the dev team laptops, could you get the attacker's C2 and the port they used?```

Flag:```ASCWG{http://C2 URL:PORT}```
```NOTE: DON'T OPEN THE REG FILE ON YOUR MACHINE```

Okay, in this challenge I wanted the players to learn about the web browser's policies registry keys, said keys can be exploited to gain persistence or execute malicious files.

Firstly, we will open the file in ```notepad+++``` or ```registry workshop```, then we will navigate to the following registry key ```\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Google\Chrome```

![2024-09-30 23_13_41-Win11 - VMware Workstation.png]({{site.baseurl}}/assets/2024-09-30 23_13_41-Win11 - VMware Workstation.png)
We will find that the key has 3 values, one of them has a weirdly looking B64 encoded string, and the other two help execute the encoded command. We decode the encoded string, we find the following

![2024-09-30 23_17_34-NVIDIA GeForce Overlay.png]({{site.baseurl}}/assets/2024-09-30 23_17_34-NVIDIA GeForce Overlay.png)

Finally, we get the flag.

Flag:```ASCWG{http://www.N0wy0ug0tth3f1ag.com:4096}```

## 3rd Challenge: Silent Persistence

![silent_presist.png]({{site.baseurl}}/assets/silent_presist.png)

Description:```an attacker managed to maintain persistence through an obscure method, our DFIR investigators weren't able to get how he did it, could you get how he gained persistence?```

Flag Format: ```ASCWG{first reg value in hex:second reg value in hex:full command} .i.e ASCWG{0xsomething:0xanother thing:C:blah blah blah and blah}```

NOTE: ```DON'T OPEN THE REG FILE ON YOUR HOST MACHINE```

Okay, in this challenge I wanted to explore some new technique which hackers use to gain persistence, which is [Image File Execution Option](https://github.com/Psmths/windows-forensic-artifacts/blob/main/persistence/image-file-execution-options.md)

If we follow, the registry keys mentioned in the github link above we will find the following artifacts
![2024-09-30 23_26_06-Win11 - VMware Workstation.png]({{site.baseurl}}/assets/2024-09-30 23_26_06-Win11 - VMware Workstation.png)

It's quite uncommon to find a global flag set for ```explorer.exe```, and related to IFEO, we need to dig deeper, but we can save the global flag value which is ```200 in hex```, we're gonna need it later.

Going for the second registry key, we find the following

![2024-09-30 23_29_19-Win11 - VMware Workstation.png]({{site.baseurl}}/assets/2024-09-30 23_29_19-Win11 - VMware Workstation.png)
2 registry keys, which correlate to the parameters needed to trigger IFEO persistence, we combine all 3 values together and we get the flag.

Flag:```ASCWG{0x200:0x1:C:\WINDOWS\system32\rundll32.exe C:\Users\test\Downloads\Taigei64.dll}```

## 4th Challenge: one access, and I'm in

![oneaccess.png]({{site.baseurl}}/assets/oneaccess.png)

Description:```an attacker managed to gain persistence on one of the devs team laptop, the persistence works each and everytime the dev authenticates, could you get the IPs and ports, which was used by the attacker?```

Flag Format: ASCWG{IP:Port-IP:Port}

In this challenge, I wanted to explore a teeny tiny bit of Linux Forensics, and as always I'm targeting the lesser common persistence techniques, since those techniques are much favored by the attackers. We are presented with 2 Zip files here, ```etc.7z```, and ```lib.7z```. Let's start investigating the ```etc``` dir first.

In the description, it's stated that the persistence mechanism is triggered each and everytime the user authenticates, looking at different config files regarding the authentication and logging in, we don't find anything that's unusual, so let's dig deeper. We need to investigate the ```pam.d``` dir.

From the first glance, nothing seems out of the ordinary, but if we took a closer look at a file named ```login```, which is a totally legitimate file. We will find an ```auth``` command which executes a ```shared object``` file or ```.so``` file.
![2024-09-30 23_52_33-login - Notepad.png]({{site.baseurl}}/assets/2024-09-30 23_52_33-login - Notepad.png)

Following the path for the ```libconnect.so``` file in the ```/lib/usr/x86_64-linux-gnu``` dir, we will find the malicious file.

Let's drop it into IDA freeware and investigate it.

First, we check the functions window, we will find a function called ```connect```

![2024-09-30 23_56_24-Windows 10 x64 - VMware Workstation.png]({{site.baseurl}}/assets/2024-09-30 23_56_24-Windows 10 x64 - VMware Workstation.png)

By navigating to the function in question we will find that it opens a backdoor to two distinctly different IPs.

![2024-09-30 23_59_21-Windows 10 x64 - VMware Workstation.png]({{site.baseurl}}/assets/2024-09-30 23_59_21-Windows 10 x64 - VMware Workstation.png)

And we get the flag.

Flag:```ASCWG{{5.42.103.173:8192-5.42.102.43:10150}```

## 5th Challenge: DarkNet Desperado: the million dark mask DF

![darknet.png]({{site.baseurl}}/assets/darknet.png)

Description:```During the arrest of the one of the criminals of impersonating the name "the broker" one of the DF investigators stumbled upon a devilish method the 
threat actor was using to deliver his malware and exfiltrate any data they need. could you get the las thing he intended to deploy or extract?```

Flag: ```ASCWG{something_something}```

In this challenge, I wanted to teach the players about a new tool which was recommended by dear friend Jimmy, this tool is called [dirDevil](https://trustedsec.com/blog/dirdevil-hiding-code-and-content-within-folder-structures) and it's used by adversaries to either deliver their malicious payloads, or for data exfiltration.

The challenge's files are basically two empty folders with zero bytes of storage, and no matter what we do there's nothing that can be done to extract any useful data from the folders(because there's none), except for their weird naming convention
![2024-10-01 00_39_21-Challenge Files.7z.png]({{site.baseurl}}/assets/2024-10-01 00_39_21-Challenge Files.7z.png)

Okay, after some googling we can find the github repo for the aforementioned [tool](https://github.com/nyxgeek/dirdevil)

We simply need to run the decoder at the same location in which the empty folders are stored, after executing the decoder script, we will find a file called ```decoded_output.txt```

![2024-10-01 00_42_43-Windows 10 x64 - VMware Workstation.png]({{site.baseurl}}/_posts/2024-10-01 00_42_43-Windows 10 x64 - VMware Workstation.png)

Inside that file, we will find the flag for this challenge.

Flag: ```ASCWG{B1!$$y_!s_N0t_0n3_0f_th3_susp3ct$}```

With this concludes this year's challenges, I hope the players learned something new and enjoyed solving  these challenges, I personally learned a lot of interesting stuff, and I'll see you all in a new blogpost.
