
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

## 2nd Challenge