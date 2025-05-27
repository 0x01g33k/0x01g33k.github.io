![2025-05-27 13_07_29-EG-CTF Final 2025 — Mozilla Firefox.png]({{site.baseurl}}/_assets/2025-05-27 13_07_29-EG-CTF Final 2025 — Mozilla Firefox.png)

Hello guys, for the first time I had the honor to write the Digital Forensics Challenges for EG|CERT's CTF, this round was wild, I wanted to create some intersting challenges with new ideas and concepts that helps in making DF investigation more relatable and shed some light on the nature of the daily of a DF investigator. So, without any further a due, let's dive into the writeup.

## First Challenge The X archives

![2025-05-27 13_21_56-Admin Panel — Mozilla Firefox.png]({{site.baseurl}}/_assets/2025-05-27 13_21_56-Admin Panel — Mozilla Firefox.png)

Description: ```During a regular investigation, our DFIR engineer stumbled upon the path to the archive, which the perpetraor claims he didn't download it or access it. Could you get the path of the file?

Flag Format: EGCTF{Drive:\example\example\file.xxx}```

In created two warmup challenges for this round, this was the first one of them. In this challenge, the idea was simple, there's a certain ```*.rar``` file that was opened on the user's machine, and the flag is the full path of that file. The players were presented with ```.hiv``` file, which they can open in ```registry explorer``` or any other specialized tool.

If we opened in in ```regsitry explorer```, we will find the hive as follows.