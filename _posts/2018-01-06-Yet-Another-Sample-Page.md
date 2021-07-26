---
title: 2020-08-15-ASCWG-Forensics-Writeup
published: true
---
---
title: Arab Security Cyber Wargames Championship CTF 2020 Forensic Write-ups
published: true
---

> These challenges were solved by **[Medoic](https://www.facebook.com/mohamedsherifhaz) and [Magdi](https://www.facebook.com/M49di)**

> ![logo](https://i.ibb.co/88K0xT7/logo.png)

> This is the write up of the for the forensics challenges in the ASC CTF qualification round. There was 3 forensics chellenges.

* let's go.

# []() Fingerprint--> 300 Pts:

* Type : Forensics
* Name : Finger Print
* Descreption : Can You Spoof My Finger Print ???
* Points : 300

> At first we get an archive file containg 7 images of fingerprints. all the images are JPG expect 1 PNG image.

> ![1](https://miro.medium.com/max/700/1*MthwHC2hSaIUPbkVDzcJVQ.jpeg)

> Checking the metadata and exif data of the jpg images we found that Slices Group Name of `3.jpg` containing challenge.jpg

> ![2](https://miro.medium.com/max/700/1*j7MuRiIG1B-u4Hw2gVvgpg.jpeg)

* This makes 3.jpg an interesting file for us

* Checking the strings of 3.jpg we find a base64 encoded string at the end of the file

> ![](https://miro.medium.com/max/700/1*j7MuRiIG1B-u4Hw2gVvgpg.jpeg)

* Decoding the base64 string we get a series of hex digits

> ![](https://miro.medium.com/max/681/1*VHrYnO8q8gnd1yf2hDepqw.jpeg)

*  Decoding the hex it appears from the magic bytes that this a Rar file

> ![](https://miro.medium.com/max/680/1*Gd4mMjBpwP84JfmfWlWuLA.jpeg)

* Saving the hex as a file we get a rar file

* Opening the rar file it appears to be password protected

### []() Cracking the Rar password:

> First thing we had to extract the password hash of the rar file using rar2john

> **sudo rar2john 3.rar > passwordhash.txt**

* Then lets attack the password using john and rockyou.txt wordlist

> **sudo john passwordhash.txt — wordlist /usr/share/wordlists/rockyou.txt — format=RAR5**

* The password decrypted is :**komputer**
* Extracting the rar file we get a text file called txt.txt
* the file then contains the flag

> **ASCWG{F0Ren$ics_I$_FUn_;)}**


# []() Meownetwork —> 300 Pts

* Type : forensics
* Name : meownetwork
* Description : A hacker managed to get into meownetwork and leaked sensitive files of their respected baord members. The hacker uses ancient floppy disk technology, however our security team managed to get a disk image of the files he leaked. Can you find out what really leaked?

* Points : 300

> We received a rar file containing disk.img

> We then mounted the img using FTK imager

> ![](https://miro.medium.com/max/576/1*k6VFTBwCvDjSGi0O1uQP6g.jpeg)

> Opening the mounted disk we find 5 images of cats

> ![](https://miro.medium.com/max/700/1*WKVVDKrFlo8Uc7TncV0Glw.jpeg)

* Checking the exif data nothing interesting appears.
* Lets then check if the images contain any hidden data using steghide and an empty password

> **steghide extract -sf 1.jpg**

* All images appear to contain a text file called --> **not_the_flag_imagenumber.txt**

> Lets extract all the text files
>
> steghide extract -sf 1.jpg
> steghide extract -sf 2.jpg
> steghide extract -sf 3.jpg
> steghide extract -sf 4.jpg
> steghide extract -sf 5.jpg

*  Opening the text files they appear to be base64 encoded
* Decoding and combining them we get a new image file

> ![](https://miro.medium.com/max/512/1*HuSkyjtKCizwvbAfESPPCA.jpeg)

* Trying steghide with empty password , extraction of data fails

> Lets try to bruteforce the password of steghide using stegcracker

> **stegcracker image.jpg /usr/share/wordlists/rockyou.txt**

* We successfully find the password: **labeba**

> ![](https://miro.medium.com/max/700/1*CSEXoAWK8t3iOVaWIEPO8g.jpeg)

* Opening the extracted file it contains the flag

> **ASCWG{F10ppy_d1$k!!_th@t’$_s0m3_n0$t@1g!a_stuFF}**

# []() The-Impossible-Dream --> 600

* level: 2
* title: The-Impossible-Dream
* Description: The notorious terrorist group known as the 10 rings got their 1337 hax0r 5 to hack into Stark Industries and steal the some sensitive files inlcuding the blueprints for the aRC Reactor second model, the hackers messed up the data badly and encrypted the files. Can you retrieve the files?


> This challenge is one hell of a ride, we have a missing headers’ file and we need to fix it first to get to the challenge.

* We start of by importing the file as raw data into audacity, it’ll identify the file as a WAV file and play the song (the song is The impossible dream for Andy Williams). however when running “file” utility it can’t recognize the file.

> We need to edit fix the magic numbers in the header

> ![](https://miro.medium.com/max/700/1*xO2qYc_TIaz59AONUKDB1A.png)

> As shown above the file is missing the RIFF.i hex values and also missing the data.i hex values which are “ 52 49 46 46” and “ 64 61 74 61” respectively.

> Fixing the header, we get nothing else. Nothing hidden in the audio itself. So we need to use deepsound.

> ![](https://miro.medium.com/max/700/1*Vb36a84G3IaFoQsm21wgZQ.png)

* And we get an img file named challenge.img, we extract it, and moving it to my kali box we find out it’s an EXT4 filesystem.

> ![](https://miro.medium.com/max/700/1*vviMnGC3Pqm7tGvuzkD4PA.png)

> Extracting the filesystem using “binwalk” utility, we get a directory called **_challenge.img.extracted/ viewing the contents of the directory we get 3 files.**

> ![](https://miro.medium.com/max/591/1*lqCS-slCvNi1QTGcrXvOIg.png)

> the pastebin.txt file has a weird string that can’t be decoded and it’s not a hash, I tried to unrar ‘ju$t_an0th3r_f!l3.rar’ but it needed a password so cracking it with “johntheripper”, we get a password **gasparin**

> ![](https://miro.medium.com/max/700/1*e6mmB_rOP6MMDvXEb25mNA.png)

> extracting the contents of the rar file we get 3 pictures that are basically memes. They serve as a rabbit hole and hold nothing important whatsoever (they actually have text files that can be extracted using “stegosuite”, I know so because I made the challenge).

* Going back to the file called Null, when using **strings** utility we find it has the flag.txt file

> ![](https://miro.medium.com/max/672/1*8dgHbme3eKeWor38K12grA.png)

> So it’s a RAR file, looking at its header the magic numbers don’t exist, so adding them will fix it **52 61 72 21**

* Attempting to extract Flag.txt we find the file is password protected

> ![](https://miro.medium.com/max/700/1*9sktmoFhhDEYFe-KBdDhKw.png)

> so now we need to get the password. We looked at the other RAR file but it’s a rabbit hole so we are left with the pastebin.txt file

> ![](https://miro.medium.com/max/688/1*NR5MupZ-7K81bkRBzSd-1w.png)

> It looks like a hash but it’s not, so we need to take a step back. Reading the description again we find some interesting stuff, “hax0r 5", “aRC reactor second model”, and last but not least encrypted. So it might be using an encryption technique like RC2 and the key is 5. Going to cyberchef

> and using RC2 decrypt
>
> ![](https://miro.medium.com/max/700/1*wZ8l6_NKFv0RGYkGsUR6eQ.png)

* we get a pastebin link **https://pastebin.com/BSZ4QRxT**

> ![](https://miro.medium.com/max/661/1*ENLs40jluGA_fpW_ur68zA.png)

> It looks like base64, but it’s actually base32, decoding it using cyberchef we get a base64 string.

> ![](https://miro.medium.com/max/700/1*Cp4rYSZ8kWG7-sagjBqtqg.png)

> Decoding that string, we get a random text.

> ![](https://miro.medium.com/max/700/1*aEhfbyqNzYpcfulyhTpSQg.png)

> It’s actually encoded using rot47

> ![](https://miro.medium.com/max/652/1*6HDcEzKgu4wS9wirDXPJ6w.png)

> @[Nottheaccounty2](https://twitter.com/Nottheaccounty2) which will take us to a twitter account, and we find a tweet that’s encoded using rot13.

> ![](https://miro.medium.com/max/601/1*bpskkZM0qlQi4f2Hs5HdPg.png)

> Decoding it will lead us to a mega drive link

> ![](https://miro.medium.com/max/700/1*5oFibAFq3ZKzvEtnjE5SPg.png)

> Going to that link will give us a file called hash.txt

> ![](https://miro.medium.com/max/700/1*7ZERT5A7RCIOCUgYn6UlGw.png)

> Cracking the hash using **crackstation** will give us the password.

> ![](https://miro.medium.com/max/700/1*W5RGSOWXZy4NhzXb3J-hww.png)

> The password is: **Password120**

> Getting back to the RAR file and extracting the flag.txt, we get the flag.

> ![](https://miro.medium.com/max/650/1*KY2Z7ZbQVBxNk-Wtl_FeHg.png)

> ASCWG{Wh0m3v3r_m@d3_Th!$_ch@113Ng3_h@s_A_L0T_oF_Fr3e_t!mE}

* Thanks For Reading.

* Cheers!