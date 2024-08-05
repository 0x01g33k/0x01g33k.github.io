Hello guys, for the fifth year in a row I've had the honor to write the Digital Forensics Challenges for Arab Security Cyber War Games 2024. This year I wanted to introduce and teach the players about new topics in DF investigation such as SCADA traffic investigation and some deeper blockchain forensics.

So let's jump right in.

## First challenge: Level 4 disaster

![2024-08-03 13_41_08-ASC Cyber WarGames — Mozilla Firefox.png]({{site.baseurl}}/assets/2024-08-03 13_41_08-ASC Cyber WarGames — Mozilla Firefox.png)

Description: ```An attacker managed to attack a critical infrastucture facility and sabotaged one of the main SCADA systems, could you find out what he did exactly?```

Flag Format:```ASCWG{Attacker's_IP:Port-system_name-changed_value}```

From the challenge's description and its name, we're investigating an ICS system, upon downloading the challenge's files, we find 2 files one of them is a pcap file and the other is ```called dump.xmpp```.
![2024-08-05 18_46_52-Kali Linux - VMware Workstation.png]({{site.baseurl}}/assets/2024-08-05 18_46_52-Kali Linux - VMware Workstation.png)

throwing command cat on the file called ```dump.xmpp```, we get some weird data, which will be useful later but we find one part of the flag, which is the system's name.

![2024-08-05 18_49_25-Kali Linux - VMware Workstation.png]({{site.baseurl}}/assets/2024-08-05 18_49_25-Kali Linux - VMware Workstation.png)

Okay, now let's open the pcap file called ```monitor.pcap```, once we open it, we find there are some weird protocol called Modbus/TCP, and by sorting them out we can understand what was going on.
![2024-08-05 18_52_48-Kali Linux - VMware Workstation.png]({{site.baseurl}}/assets/2024-08-05 18_52_48-Kali Linux - VMware Workstation.png)

By viewing each packet's contents, we find that the attack first read the value in register number 3, which was 1024.
![2024-08-05 18_55_42-Kali Linux - VMware Workstation.png]({{site.baseurl}}/assets/2024-08-05 18_55_42-Kali Linux - VMware Workstation.png)

Then the attacker changed the value to 8000 as shown in this packet.
![2024-08-05 18_58_30-Kali Linux - VMware Workstation.png]({{site.baseurl}}/assets/2024-08-05 18_58_30-Kali Linux - VMware Workstation.png)

The flag required some parts to be pieced together and they are the attacker's IP, the port, the system's name, and the changed value. Now we have all of them.

Flag:```ASCWG{192.168.159.128:502-WaterTank-8000}```

## Second challenge: We got a new contract for you.
![2024-08-03 13_42_44-ASC Cyber WarGames — Mozilla Firefox.png]({{site.baseurl}}/assets/2024-08-03 13_42_44-ASC Cyber WarGames — Mozilla Firefox.png)

Description:```During a transaction on a newly generated smart contract, one of the clients noticed there's something wrong going, the coins weren't sent fully, after proper investigation turned out the client fell victim to address poisoning attack, we were only able to get a transaction hash for the contract, 
could you get the address of the attacker's wallet from the smart contract?
Transaction hash: 0x21e3ace7c88f9476f70fe3c89d5baedbb342491a63c0d7163e158cd74d9058b3```

Flag Format: ```ASCWG{Address}```

I want give a shout out to my dear friend and colleague [Khaled ElSergany](https://www.linkedin.com/in/khaled-elsergany-5676b8261/) who helped me shape my idea into a proper challenge. All love, ma homie :D.

Let's start by viewing the transaction hash on sepolia test network.

![2024-08-05 19_25_19-Sepolia Transaction Hash (Txhash) Details _ Etherscan — Mozilla Firefox.png]({{site.baseurl}}/assets/2024-08-05 19_25_19-Sepolia Transaction Hash (Txhash) Details _ Etherscan — Mozilla Firefox.png)

Okay, so we are looking for a contract which was used in an address poisoning attack, and we are looking for a wallet's address which was the money sent to. This sounds super nice.

First, we click on the contract's address, and we will be directed to page which includes all the transactions happened on this contract.

![2024-08-05 19_38_48-Contract Address 0xe4f4f8cde91442a0716622cf6d8353812c1ec44f _ Etherscan — Mozill.png]({{site.baseurl}}/assets/2024-08-05 19_38_48-Contract Address 0xe4f4f8cde91442a0716622cf6d8353812c1ec44f _ Etherscan — Mozill.png)

From here, we will click on the tab called contract, and we will view the opcode for the contract itself.

![2024-08-05 19_42_41-Contract Address 0xe4f4f8cde91442a0716622cf6d8353812c1ec44f _ Etherscan — Mozill.png]({{site.baseurl}}/assets/2024-08-05 19_42_41-Contract Address 0xe4f4f8cde91442a0716622cf6d8353812c1ec44f _ Etherscan — Mozill.png)

We will scroll down just a lil bit and we will see an opcode which pushes data to an address, and voila that's the address we are looking for.

![2024-08-05 19_44_27-Contract Address 0xe4f4f8cde91442a0716622cf6d8353812c1ec44f _ Etherscan — Mozill.png]({{site.baseurl}}/assets/2024-08-05 19_44_27-Contract Address 0xe4f4f8cde91442a0716622cf6d8353812c1ec44f _ Etherscan — Mozill.png)

Flag:```ASCWG{0xa7a85e048825c8359cd836185df877bf49ab65df}```

## Third Challenge: I know your secrets

![2024-08-03 13_42_29-ASC Cyber WarGames — Mozilla Firefox.png]({{site.baseurl}}/assets/2024-08-03 13_42_29-ASC Cyber WarGames — Mozilla Firefox.png)

Description:```our beloved hacker who hacked the SCADA system in the earlier challenge, got arrested and while examining their router, our investigators found out it was compromised as well. Could you get the IP and port of the attacker who hacked the router?```

Flag Format:```ASCWG{IP:Port}```

Okay, I wanted to introduce the players to firmware forensics, so I created a backdoor in a router's firmware, and they need to investigate the files to find the backdoor.

Upon downloading the challenges' file, we find only a .bin file. Let's extract it using binwalk

![2024-08-05 20_13_09-Kali Linux - VMware Workstation.png]({{site.baseurl}}/assets/2024-08-05 20_13_09-Kali Linux - VMware Workstation.png)

Okay, there are lots of files, which to investigate is like finding a needle in a haystack, a quick way to naivgate to which file was edited lastly is to sort them by date, which is what we're gonna do using this command ```ls -ltr```, we will find the last modified file in the end of the list.

![2024-08-05 20_16_39-Kali Linux - VMware Workstation.png]({{site.baseurl}}/assets/2024-08-05 20_16_39-Kali Linux - VMware Workstation.png)


By reading the content of this file, we find there's a new class added and it has a base64 encoded string at the end of it, we decode it only to find there's another base64 ecoded string as well, we decode it again, only to find the backdoor code and its respective IP and port.

![2024-08-05 20_19_16-Kali Linux - VMware Workstation.png]({{site.baseurl}}/assets/2024-08-05 20_19_16-Kali Linux - VMware Workstation.png)

Flag:```ASCWG{41.129.186.63:50158}```

## Fourth Challenge: You can run, but you can't land 1

![2024-08-03 13_42_05-ASC Cyber WarGames — Mozilla Firefox.png]({{site.baseurl}}/assets/2024-08-03 13_42_05-ASC Cyber WarGames — Mozilla Firefox.png)

Description:```During an investigation, one of the team members stumbled upon weird behavior in the registry regarding a task. Could discover what's wrong with this machine?```

Flag Format:```ASCWG{name of the weird task:file hash}```

Okay, we are presented with 2 reg files one if for Hot_Key_Current_User (HKCU), and the other is for Hot_Key_Local_Machine (HKLM).

According to the description, we need to find something related to task, it's quite obvious we are looking for scheduled tasks, so let's start investigating them.

PS. To all the players, veterans or new and especially the new ones. WE NEVER OPEN A REG FILE ON OUR HOST MACHINE, no one is sane enough to do so, when you wreck your machine don't blame anyone but yourselves. 

We will start with HKLM file, we will navigate to the following key, ```\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree```, there we will find a task called ```RunDownloadAndExecuteDLL```.

![2024-08-05 20_44_49-Windows 10 x64 - VMware Workstation.png]({{site.baseurl}}/assets/2024-08-05 20_44_49-Windows 10 x64 - VMware Workstation.png)

Perfect, now we need to know what it does exactly, so we will navigate to the following key in HKCU reg file, ```\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Run```.

Okay, so we find a task called ```DownloadDLL```, which invokes base64 powershell encoded script.

![2024-08-05 20_46_59-Windows 10 x64 - VMware Workstation.png]({{site.baseurl}}/assets/2024-08-05 20_46_59-Windows 10 x64 - VMware Workstation.png)


Upon decoding the script, we find that it downloads a dll file and runs it in memory.


![2024-08-05 20_52_02-NVIDIA GeForce Overlay.png]({{site.baseurl}}/assets/2024-08-05 20_52_02-NVIDIA GeForce Overlay.png)

I wanted to teach the players that even discord could be used as a dropper to evade detection.

Now we download the file. For some reason, discord stopped downloading the file directly, so the work around was to send the link in chat to anyone and it'll redirect the link to the designated file and allow the players to download it.

Now we have the task's name and the file, let's hash it and get the flag.

![2024-08-05 20_55_18-Windows 10 x64 - VMware Workstation.png]({{site.baseurl}}/assets/2024-08-05 20_55_18-Windows 10 x64 - VMware Workstation.png)


Flag:```ASCWG{RunDownloadAndExecuteDLL:522B754BC18F4C89B493F630158A55EE733CDC11084F76B426547FA1853799E2}```

## Fifth Challenge: You can run but you can't land 2

![2024-08-03 13_41_45-ASC Cyber WarGames — Mozilla Firefox.png]({{site.baseurl}}/assets/2024-08-03 13_41_45-ASC Cyber WarGames — Mozilla Firefox.png)


Description:```From the previous REG files, could you get the IP and port of the attacker's machine?```

Flag Format:```ASCWG{IP:Port}```

Okay, now we need to reverse the DLL file to get the IP and port of the attacker, let's fire up IDA freeware and get to it (which is the correct and intended way)

Upon opening the file in IDA, we navigate to imports tab, and we find ```inet_addr```

![2024-08-05 20_59_15-Windows 10 x64 - VMware Workstation.png]({{site.baseurl}}/assets/2024-08-05 20_59_15-Windows 10 x64 - VMware Workstation.png)

By following it and converting view to graph, we find that it XORs the IP's octets with 0xAA.

![2024-08-05 21_03_26-Windows 10 x64 - VMware Workstation.png]({{site.baseurl}}/assets/2024-08-05 21_03_26-Windows 10 x64 - VMware Workstation.png)

As for the port we go follow the graph to find it.

![2024-08-05 21_05_48-Windows 10 x64 - VMware Workstation.png]({{site.baseurl}}/assets/2024-08-05 21_05_48-Windows 10 x64 - VMware Workstation.png)

Converting these values to decimal, we get the flag.
![Untitled.jpg]({{site.baseurl}}/assets/Untitled.jpg)

Flag:```ASCWG{41.33.50.42:4096}```
