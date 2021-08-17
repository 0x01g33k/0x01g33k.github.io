---
published: true
---
For the second year in a row I had the honor to write the DF challenges for ASCWG 2021
 I've created 5 challenges for this year, some of them were solved in an unintended way, and some were solved the way I designed them to be.
 
 <p>1st Challenge: Warmup-01</p>
 ![]({{site.baseurl}}/assets/2021-07-30%2018_39_54-Window.png)
 
Description: ```let's take things slowly, what's the first thing to do when you get a pic file that has a hidden string in it, in order to extract that string?```
 
 The challenge is extremely easy, looking at the file we get nothing useful
![]({{site.baseurl}}/assets/WhatsApp%20Image%202021-07-30%20at%202.28.34%20AM.jpeg)


I added some memes to the challenges :"D

Using ```exiftool``` we find some interesting stuff
![2021-07-31 13_24_51-Window.png]({{site.baseurl}}/assets/2021-07-31 13_24_51-Window.png)

Decoding the b64 string we get the flag.
![]({{site.baseurl}}/assets/2021-07-31%2013_27_10-Window.png)

Flag: ```ASCWG{1337_DF1R_1nv3stg@t0R}```

<p>2nd Challenge:Warmup-02</p>

![2021-07-30 18_39_35-Window.png]({{site.baseurl}}/assets/2021-07-30 18_39_35-Window.png)

Description:```Taking things a bit higher, lets try to get the flag from that hex.```

Reading the content of the file called warmup-02 we get a hexdump for a rar file. 

![2021-07-31 13_31_46-Window.png]({{site.baseurl}}/assets/2021-07-31 13_31_46-Window.png)

Converting the hexdump to a rar file and bruteforcing, the rar password, which is ```cassandra```, we get the flag. 

![2021-07-31 13_33_14-Window.png]({{site.baseurl}}/assets/2021-07-31 13_33_14-Window.png)

Flag: ```ASCWG{H3xDump_1s_EZ}```

<p>3rd Challenge: noobie sysadmin</p>

![]({{site.baseurl}}/assets/2021-07-30%2018_38_33-Window.png)

Description: ```A sloppy sysadmin accidently deleted some files from our lulzcorp servers, can you recover the deleted file?```

This challenge could be solved in two different methods, the first way is to recover the deleted file via vhd data recovery program. The second method is to mount the vhd disk and recover the deleted file using any data recovery program. I will explain each of these methods.

Method 1: We download a program called ```Kernel for VHD```, which recovers the deleted data from VHD disks.

![]({{site.baseurl}}/assets/2021-08-01 01_14_01-Window.png)

Then we open the vhd disk in the program, and choose extensive scan. 

Going through the steps of the program, we get the deleted files. 
![]({{site.baseurl}}/assets/2021-08-01 01_16_28-Window.png)

Recovering that file, and opening it we get a base64 encoded string. 
![]({{site.baseurl}}/assets/2021-08-01 01_18_39-Window.png)

Base64 string: ```base64
UmFyIRoHAQAzkrXlCgEFBgAFAQGAgAB6Vf0IVQIDPLAABJwAICK6s9qAAwAIRmxhZy50eHQwAQADD532NozZb1ZrgfFV3qU6mBTY3v9TSaZgxr/JrNdVfjTOMSZMYCYwUH0WRQ9bCgMCV5pOPEdl1wGtdkGJEOMt7FX420NCP8UCJzvVrOw8pGZq/BoJnk2YMyD5oIcsrKFygUQdwW+yJ8wdd1ZRAwUEAA==```

Decoding that string using ```cyberchef```, we get some data that shows it's a rar file.
![]({{site.baseurl}}/assets/2021-08-01%2001_21_02-Window.png)

Downloading that file and bruteforcing its password, which is ```sage123```, and we are able to extract the flag.

![]({{site.baseurl}}/assets/2021-08-01 01_23_23-Window.png)

Flag:```ASCWG{VHD_F0rens1cs_ar3_Fun}```

Method 2: We can recover the deleted file by mount the VHD disk and recover the file, using any data recovery program.

Using disk management we can mount the VHD disk ![]({{site.baseurl}}/assets/2021-08-01 01_25_39-Window.png)

then after mounting the disk we recover the deleted file and continue with the same steps above.

<p> 4th Challenge: The Possible Dream</p>

![]({{site.baseurl}}/assets/2021-07-30%2018_39_00-Window.png)


Description: ```A hacker is intending on leaking some data to his cyber gang, he couldn't evade the security solutions in the company. So he's leaking them via unconventional methods. He also encrypted the file he intends to exfiltrate with a weak encryption scheme that could be bruteforced. Can you find out what he wants to exfiltrate?```

This challenge could be solved in two methods, the first is the intended method, the second is unintended. I will explain both methods. 

Method 1: 
We look at the file itself, and we notice a meme :"D

![]({{site.baseurl}}/assets/2021-08-01%2001_29_49-Window.png)

Well to cut things short, there's no hint that we can get anything useful by using spectrogram or any other common stego method. We can only use ```steghide``` to extract the hidden file.

![]({{site.baseurl}}/assets/2021-08-01 01_35_04-Window.png)

Now we have an encrypted file and we don't know what to do with it, the purpose of this challenge was to make the players write a script to extract LSB and get the key. 

the code I wrote to extract the key: 

```python
import wave
song = wave.open("My_Story.wav", mode='rb')
frame_bytes = bytearray(list(song.readframes(song.getnframes())))

extracted = [frame_bytes[i] & 1 for i in range(len(frame_bytes))]
string = "".join(chr(int("".join(map(str,extracted[i:i+8])),2)) for i in range(0,len(extracted),8))
decoded = string.split("###")[0]

print("Sucessfully decoded: "+decoded)
song.close()
```

Using that script we are able to extract the key. 

![]({{site.baseurl}}/assets/2021-08-01 01_41_43-Window.png)

Now going to ```cyberchef``` with the encrypted file and the key, which is ```05``` in hex, We are able to extract the encrypted file

![]({{site.baseurl}}/assets/2021-08-01%2001_44_38-Window.png)


Downloading the file and bruteforcing the password, which is ```musicislife```. We get the flag.

Flag: ```ASCWG{Mu$1c_$t3g0_@r3_C0o1}```

Method 2: 

Just open the file with in cyberchef and use XOR bruteforce recipe, and you will get the correct key. :"D

![]({{site.baseurl}}/assets/2021-08-01%2001_46_52-Window.png)


<p>5th Challenge: Registry Nightmare</p>

![]({{site.baseurl}}/assets/2021-07-30%2018_39_18-Window.png)
 
Description: ```A hacker was able to gain access to the CEO's machine, he gained access to the machine by using an outdated vulnerable version of XYZ program, the program was used a lot by the CEO. The hacker was able to gain access to the computer physically and was about to get the files he needed but he was caught, can you recover the flag from the reg file?
NOTE: You'll need a key and IV to decrypt the flag, both of which are in front of you when you find what you're looking for.```

This challenge was designed to be realistic, I wanted the participants to learn and read a thing or two about registry forensics. 

The challenge had one rabbit hole, since it was hard. 

To start investigating some reg files regarding an incident, we look into some famous keys, like RunMRU, and MountedDevices. 

The rabbit hole was in RunMRU path: 
```HKEY_CURRENT_USER\software\microsoft\windows\currentversion\Explorer\RunMRU```

NOTE: some people faced some hardship opening the file, they needed to open it using ```notepad++```.

![]({{site.baseurl}}/assets/2021-08-01%2001_53_41-Window.png)

Decoding the base64 value, we get the key, the IV, and the encrypted string, we get the rabbit hole.

![]({{site.baseurl}}/assets/2021-08-01%2001_55_41-Window.png)

![]({{site.baseurl}}/assets/2021-08-01 01_56_46-Window.png)

Now we go to the second registry key which is MountedDevices. 

Path: ```HKEY_LOCAL_MACHINE\SYSTEM|MountedDevices```

![]({{site.baseurl}}/assets/2021-08-01%2001_58_58-Window.png)

And same as before, we decode the base64 string, and then we decrypt the encrypted string and we get the flag.

![]({{site.baseurl}}/assets/2021-08-01%2002_00_18-Window.png)

Flag: ```ASCWG{R3g1stry_F0r3ns1c$_1s_Sup3r_C0O1}```

I'd like to give a big shout out to my dear friend [Mohammed Hassan](https://0xmohammed.github.io) for giving me a new method to solve the challenge without the need to use ```notepad++```

Mohammed's method: ```strings -e l challenge.reg |grep 'MountedDevice' -A5```

And this concludes the DF challenges for ASCWG 2021 Qual Round, I hope the participants have enjoyed solving these challenges and learned something new, and I will see you all in the finals.
