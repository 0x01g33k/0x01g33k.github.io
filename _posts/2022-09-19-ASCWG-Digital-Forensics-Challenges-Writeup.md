---
published: true
---
Hello guys, for the 3rd year in a row I had the honor of writing the digital forensics challenges for Arab Security Cyber War Games 2022, this year I wanted the challenges a bit more realistic and level up the competition's atmosphere. I created 5 challenges for the final round, 2 of them are warmup challenges. Without any further a due, let's get into things.

1st Warmup Challenge: 
![Warmup-1.PNG]({{site.baseurl}}/assets/Warmup-1.PNG)

Description:```someone encrypted the drive, could you get the key?```

In this challenge, it's quite clear we are dealing with some sort of disk encryption keys stored in memory. 
![2022-09-24 23_37_40-Kali Linux - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-24 23_37_40-Kali Linux - VMware Workstation.png)
now we need to use the provided profile, which is ```Win7SP1x64``` and use bitlocker plugin to get the bitlocker key. 

![2022-09-25 00_09_08-Kali Linux - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 00_09_08-Kali Linux - VMware Workstation.png)

Flag: ```ASCWG{1dc0023cdaef9d70a7986cdd71749743:50e6440e15baac61bbabd1b8dae3d325}```

2nd Warmup Challenge:

![Warmup-2.PNG]({{site.baseurl}}/assets/Warmup-2.PNG)

Description:```During the data acquisition of a cyber criminal's sim card, the extracted file was damaged, could you repair the file and get their account number?```

Okay, this is a new challenge I wanted the participants to learn about SIM card forensics, Oxygen Forensics tool, and how to think and act if they got a corrupted case file during a real DF investigation.

Upon opening the ```.ofbx``` file in ```oxyviewer.exe```, we get a notification saying the file is corrupted. 
![2022-09-25 00_16_43-Win10 Pro - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 00_16_43-Win10 Pro - VMware Workstation.png)

Okay, we need to take a step back to fix the file, so we need to open it in a hex editor.
![2022-09-25 00_18_00-Win10 Pro - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 00_18_00-Win10 Pro - VMware Workstation.png)

At first glance, we notice the file is missing its magic bytes, if we looked for these specific magic bytes online, we won't find them. However, if we attempted to search _differently_, we can look up similar files with the same magic bytes. 

The file in question is _```zip archive```_ and its magic bytes are ```50 4b 03 04```, additionally we can look up the header of the file, which will provide us with the first values, and they are ```PK..```
![2022-09-25 00_28_40-Win10 Pro - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 00_28_40-Win10 Pro - VMware Workstation.png)
![2022-09-25 00_31_03-Win10 Pro - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 00_31_03-Win10 Pro - VMware Workstation.png)

Now after fixing the file, we need to use the ICCID number to get the account number linked to this SIM Card, the ICCID number is ```89410116224705323469```. Now we supply that number into any ICCID tracker, such as [phone.fyicenter.com](http://phone.fyicenter.com/1155_ICCID_SIM_Card_Number_Checker_Decoder.html)

Then we get the account number which is ```1622470532346``` 
![2022-09-25 00_35_41-Win10 Pro - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 00_35_41-Win10 Pro - VMware Workstation.png)

Flag:```ASCWG{1622470532346}```

3rd Challenge:```Crash Into The Deep```

![Crash into the deep.PNG]({{site.baseurl}}/assets/Crash into the deep.PNG)

Description:```our new trainee was practicing on BoF, he accidentally crashed the application and cannot start it again, could you get the return address of the wow64ldrinitialize?```

Okay, in this challenge I wanted the participants learn about utilizing windbg, or any debugger of their preference. 
We start by analyzing the crash dump, first, we need to download or import the symbols, we do so by typing this command ``` srv*https://msdl.microsoft.com/download/symbols```, then we start analyzing the file.
![2022-09-25 00_45_45-Win10 Pro - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 00_45_45-Win10 Pro - VMware Workstation.png)

Now, let's get down to the real business, the target here is to get the return address of ```wow64ldrinitialize```, we simply need to dump the stack. We type the following command ```kb```.
![2022-09-25 00_47_44-Win10 Pro - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 00_47_44-Win10 Pro - VMware Workstation.png)

Flag:```ASCWG{cf6039c7}```

4th Challenge: rooted

![Rooted.PNG]({{site.baseurl}}/assets/Rooted.PNG)

Description:```A hacker was able to get implant a backdoor into the OS of the CEO's Laptop, could you recover the base of the directory table?```

This challenge could be divided into at least 3 challenges ranging in difficulty, yet it wouldn't be hard level for no reason, this is a Linux memory dump for an inserted rootkit, the rootkit's name was chosen to hide in plain sight and make it hard for investigators to notice it quickly, (to be honest I wanted to make that challenge much much harder but it wouldn't have been solved if I did so :'D)
first, we need to build the profile of the memory dump, I added a note to the description during the competition stating that it's a ```Ubuntu 18.04.6 OS```, and its kernel is ```5.4.0-125-generic```.

We build the profile using the following commands, then we add the newly created zip file to the following path: ```volatility/plugins/overlays/linux/```

commands: ```cd volatility/tools/linux```, ```make```, ```sudo zip volatility/plugins/overlays/linux/Ubuntu1804-0504.zip tools/linux/module.dwarf /boot/System.map-5.4.0-122-generic```

NOTE: there might be an error when trying to make the module.dwarf file, we simply need to download the package which will enable us to create the module in the first place, which is ```dwarfdump```

![2022-09-25 01_26_50-Ubuntu-Test - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 01_26_50-Ubuntu-Test - VMware Workstation.png)

Then we can start analyzing the memory dump.

![2022-09-25 01_41_54-Ubuntu-Test - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 01_41_54-Ubuntu-Test - VMware Workstation.png)

The first thing we notice, is the weird naming, almost similar to the real ```ahci``` kernel module, and there's already a legitimate ```ahci``` kernel mod present and running, okay let's see if there are any hidden processes, using ```linux_psxview``` plugin.

Comparing the output of ```linux_pslist``` or ```linux_pstree``` with the output from ```linux_psxview```, we can see there's indeed a hidden process, which its PID is 2036.

![2022-09-25 01_48_01-Ubuntu-Test - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 01_48_01-Ubuntu-Test - VMware Workstation.png)
![2022-09-25 01_48_23-Ubuntu-Test - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 01_48_23-Ubuntu-Test - VMware Workstation.png)

Now we need to investigate further, we use ```linux_volshell``` plugin.
![2022-09-25 01_51_16-Ubuntu-Test - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 01_51_16-Ubuntu-Test - VMware Workstation.png)

Then we use cc() command to investigate the process ins question, we type the following command.
```cc(offset=0x0000000011bd2f80, pid=2036, name=None, physical=False)```
![2022-09-25 01_52_29-Ubuntu-Test - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 01_52_29-Ubuntu-Test - VMware Workstation.png)

Flag:```ASCWG{0x21112000}```

5th Challenge: More than meets the eye

![more than meets the eye.PNG]({{site.baseurl}}/assets/more than meets the eye.PNG)

Description:```this weird FS contains more than meets the eye, could you find what it is?```

Okay, this challenge is sneaky, this filesystem is only 5MB in size, if we went after it in normal methods we will only dig deeper and deeper into the rabbit hole, the key to solving this challenge is in its name. 

We open the file in any hex editor, first thing after the magic bytes we notice two sets of hex values.
Set no.1:

![2022-09-25 01_58_55-Ubuntu-Test - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 01_58_55-Ubuntu-Test - VMware Workstation.png)

Set no.2:
![2022-09-25 01_59_56-Ubuntu-Test - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 01_59_56-Ubuntu-Test - VMware Workstation.png)

Decrypting the first set from hexdump using cyberchef, we discover it's a base64 encoded string, attempting to decode won't result in anything useful.

We need to convert the hex values to the original base64 encoded value.

![2022-09-25 02_11_26-Ubuntu-Test - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 02_11_26-Ubuntu-Test - VMware Workstation.png)

Converting the second set from hex, as its header has PK; then it's a zip file. 
We download the file, only to find it needs a key. Now on to the ```mem_file``` itself.

Using ```fdisk``` utility, we get the partition table and its details, the key is the disk identifier ```0x7197d620```
![2022-09-25 02_23_18-Ubuntu-Test - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 02_23_18-Ubuntu-Test - VMware Workstation.png)

Now open the file and get the key.
![2022-09-25 02_27_31-Ubuntu-Test - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 02_27_31-Ubuntu-Test - VMware Workstation.png)

Now to decrypt the encrypted text.

![2022-09-25 02_24_36-Ubuntu-Test - VMware Workstation.png]({{site.baseurl}}/assets/2022-09-25 02_24_36-Ubuntu-Test - VMware Workstation.png)

Flag:```ASCWG{N0t_@11_f!l3_Sy$tems_@r3_th3_s@m3}```

And this concludes the final round challenges, I hope the participants learned something new and I hope to see you again next year insha'Allah.
