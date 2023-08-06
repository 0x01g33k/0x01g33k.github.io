---
published: false
---
Hello guys, for the fourth year in a row, I've had the honor to write the Digital Forensics for Arab Security Cyber War Games 2023. This year, I wanted to target different vectors such as blockchain and some deep dive memory forensics. So without any further adue, let's get into the writeup :D. 

## First challenge:I thought I could trust you.
![WhatsApp Image 2023-08-05 at 2.26.55 PM.jpeg]({{site.baseurl}}/assets/WhatsApp Image 2023-08-05 at 2.26.55 PM.jpeg)


Description:```A hacker managed to place a persistent malicious DLL file in a very and highly protected registry hive, could you get the path and the name of the malicious file from the memory dump?```

Difficulty:```Easy```

Flag Format:```ASCWG{registry key without HKCU or HKLM-name of the dll}```

From the description and the challenge's name, we are looking for a certain registry hive that can't be manipluated that easily, and windows itself relies upon to store the most common DLLs, basically we're looking for known DLLs registry hive. 

Looking at the memdump, we find out it's a .vmem file, which is for VMware machines, volatility2 could handle such files, but since it can't identify the later releases of windows past 1904 release, it won't provide the correct output needed, so we're gonna use volatility3.

```Actually I meant for that to happen since volatility2 is becoming more and more obsolete, and yes it does its job, but we're starting to rely and use volatility3 so I wanted the players to learn how to use it.```

Checking volatility3 plugins we find this plugin ```windows.registry.printkey.PrintKey```, which can help us print the registry keys. 

To provide more visibility we use the plugin's flag ```--recurse```.
![2023-08-06 13_30_55-Ubuntu22.04 - VMware Workstation.png]({{site.baseurl}}/assets/2023-08-06 13_30_55-Ubuntu22.04 - VMware Workstation.png)

KnownDLLS being one of the crucial registry hives, it can't be manipulated that easily, and that DLL's name isn't that normal, hence we get the flag.

Flag:```ASCWG{\SYSTEM\ControlSet001\Control\Session Manager\KnownDLLs-Taigei64.dll}```

## Second Challenge:root of evil.

![WhatsApp Image 2023-08-05 at 2.26.55 PM(1).jpeg]({{site.baseurl}}/assets/WhatsApp Image 2023-08-05 at 2.26.55 PM(1).jpeg)

Description:```During our investigation in the memory dump from the first challenge, our investigator stumbled upon a signed malicious driver that wes deployed on the machine, could you get the address of the windows API functions, which the driver was using?```

Difficulty:```Easy```

Flag Format:```ASCWG{Address1-Address2}```

Okay, so we need to go back to the memdump and see if it has any malicious drivers, using volatility3. We will use plugin called ```windows.modscan.ModScan```, so let's see what drivers we have.

![2023-08-06 13_45_10-Ubuntu22.04 - VMware Workstation.png]({{site.baseurl}}/assets/2023-08-06 13_45_10-Ubuntu22.04 - VMware Workstation.png)

We find that naughty driver hiding in what it seems to be a normal place, basic hiding in plain sight 101 for malwares and malicious drivers. 

```This driver is in fact a signed malicious driver part of the blackcat ransomware gang malwares```

We use the same plugin but provide its additional flag, which is ```--dump```, to dump all the drivers.

![2023-08-06 13_48_39-Ubuntu22.04 - VMware Workstation.png]({{site.baseurl}}/assets/2023-08-06 13_48_39-Ubuntu22.04 - VMware Workstation.png)

We then open the dump for the driver in ghidra or any decompiler (I prefer using ghidra) or debugger you choose, and we get the functions it's importing.



