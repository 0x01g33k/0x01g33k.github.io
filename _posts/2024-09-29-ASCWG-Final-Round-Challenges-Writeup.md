
Hello guys, for  the 5th year in a row I've had the honor to write the challenges for Arab Security Cyber WarGames (ASCWG). This is the writeup for the final round's challenges for this year's edition. Without any further adue, let's jump right in.

## 1st Challenge: ```Validate what you wrote```

![validate.png]({{site.baseurl}}/assets/validate.png)

Description:```During an investigation, one of our DF investigators stumbled upon a file which the suspect claims is a forged, we had the original file, but we need the hash of the file which the investigator found. Could you get the file's hash?

NOTE: hash is in an uppercase format```

TL;DR: Okay, so in this challenge I wanted to teach the players about a new artifact related to ```Windows 11```, as of Win11, we don't need to save any file we open in notepad since it stores all of the file's content and data in a ```bin file```, which is stored on the drive itself, the file's content is loaded each and everytime we open notepad, the bin file has a lot of data regarding each and every file we open in notepad, one of the important artifacts is the hash value for the opened files, and that was the flag in this challenge. You can understand the internals of the artifact perfectly in this eloquent blogpost https://u0041.co/posts/articals/exploring-windows-artifacts-notepad-files/ made by [AbdulRhman Alfaifi](https://twitter.com/A__Alfaifi)

I wanted the players to extract the hash of the file which I opened in notepad. So, first thing people usually do is run ```strings``` on the file. Upon doing so we find the following:
