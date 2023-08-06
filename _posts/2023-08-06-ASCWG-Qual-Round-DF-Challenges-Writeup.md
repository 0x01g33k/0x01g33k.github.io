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

After import the .dmp file, we navigate to the imports folder, and we find it imports to functions from HAL.DLL, and ntoskrnl.exe

![2023-08-06 16_57_40-Ubuntu22.04 - VMware Workstation.png]({{site.baseurl}}/assets/2023-08-06 16_57_40-Ubuntu22.04 - VMware Workstation.png)

We click any of them and we get their address.

![2023-08-06 16_58_55-Ubuntu22.04 - VMware Workstation.png]({{site.baseurl}}/assets/2023-08-06 16_58_55-Ubuntu22.04 - VMware Workstation.png)


Flag:```ASCWG{f8044b06c093-f8044b06c07b}```

## Third Challenge: Track a coing from your crypto O valley of plenty

![WhatsApp Image 2023-08-05 at 2.26.55 PM(2).jpeg]({{site.baseurl}}/assets/WhatsApp Image 2023-08-05 at 2.26.55 PM(2).jpeg)

Description:```During the investigation in one of the cases, our forensics analysts were tasked with tracing the following transaction hash which occured on a trial network, and get the first address who sent money. Could you find that address?

Transaction Hash:0xa150ff06619b927cc323f16984a679a24b07265f1c1a664f1c729177929cebae```
Difficulty:```Medium```

First and foremost, before going into any details about this challenge and the next one, as they are both related to blockchain and cryptocurrency, I'd like to give a huge shoutout to my dear friend and colleague. One of the few pioneers in the world of cryptocurrency in Egypt and a great addition to our DF team: [Khalid ElSergany](https://www.linkedin.com/in/khaled-elsergany-5676b8261/) as he helped me create these challenges. 

In this challenge we are presented with a transaction hash, and in the description it's stated that the network is trial network. Okay, let's get digging. 

There are some test networks that make us try different techniques with crypto to avoid losing money in real in transactions, chief among them is [sepolia etheruem](https://sepolia.etherscan.io/), and this is what was meant by trial network in the description.

Okay, now let the actual show begin. We will submit the transaction has we are provided with, and we get the following.

![2023-08-06 17_13_37-Sepolia Transaction Hash (Txhash) Details _ Etherscan — Mozilla Firefox.png]({{site.baseurl}}/assets/2023-08-06 17_13_37-Sepolia Transaction Hash (Txhash) Details _ Etherscan — Mozilla Firefox.png)

Let's take a step back to know where are heading, we are tasked with tracing this transaction to get the first address sent money to the receiver's wallet. Sounds easy :'D

By simply clicking on the address, which received the coin, we get all the transactions that occured from and to that wallet. 

![2023-08-06 17_16_59-Sepolia Transactions Information _ Etherscan — Mozilla Firefox.png]({{site.baseurl}}/assets/2023-08-06 17_16_59-Sepolia Transactions Information _ Etherscan — Mozilla Firefox.png)

We scroll alllllllllllll the way down and we find that first transaction we are tacing.
![2023-08-06 17_18_12-Sepolia Transactions Information _ Etherscan — Mozilla Firefox.png]({{site.baseurl}}/assets/2023-08-06 17_18_12-Sepolia Transactions Information _ Etherscan — Mozilla Firefox.png)

Clicking on that transaction, we find the address which sent 0.5ETH.

![2023-08-06 17_19_09-Sepolia Transaction Hash (Txhash) Details _ Etherscan — Mozilla Firefox.png]({{site.baseurl}}/assets/2023-08-06 17_19_09-Sepolia Transaction Hash (Txhash) Details _ Etherscan — Mozilla Firefox.png)

Flag:```ASCWG{0xEDaf4083F29753753d0Cd6c3C50ACEb08c87b5BD}```

## Fourth Challenge: We have a new contract for you

Description:```During further investigation on the wallet that received money that we found from the transaction hash in the previous challenge, we noticed there's a token that was generated, and used to send money as part of money laundring. Could find the last address that received tokens from this contract, and the address of the smart contract?

Flag Format: ASCWG{contract address: last wallet address}```

Okay, this is a LIL bit tricky 



