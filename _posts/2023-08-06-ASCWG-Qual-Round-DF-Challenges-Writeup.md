---
published: true
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

I also added a meme for anyone who thought they could cut through the memdump and just use strings with ASCWG :'D.

the meme: ![ASCWG-Flag.png]({{site.baseurl}}/assets/ASCWG-Flag.png)


## Third Challenge: Track a coin from your crypto O valley of plenty

![WhatsApp Image 2023-08-05 at 2.26.55 PM(2).jpeg]({{site.baseurl}}/assets/WhatsApp Image 2023-08-05 at 2.26.55 PM(2).jpeg)

Description:```During the investigation in one of the cases, our forensics analysts were tasked with tracing the following transaction hash which occured on a trial network, and get the first address who sent money. Could you find that address?```

```Transaction Hash:0xa150ff06619b927cc323f16984a679a24b07265f1c1a664f1c729177929cebae```

Difficulty:```Medium```

First and foremost, before going into any details about this challenge and the next one, as they are both related to blockchain and cryptocurrency, I'd like to give a huge shoutout to my dear friend and colleague. One of the few pioneers in the world of cryptocurrency investigation in Egypt and a great addition to our DF team: [Khalid ElSergany](https://www.linkedin.com/in/khaled-elsergany-5676b8261/) as he helped me create these challenges. 

In this challenge we are presented with a transaction hash, and in the description it's stated that the network is trial network. Okay, let's get digging. 

There are some test networks that make us try different techniques with crypto to avoid losing money in real in transactions, chief among them is [sepolia etheruem](https://sepolia.etherscan.io/), and this is what was meant by trial network in the description.

Okay, now let the actual show begin. We will submit the transaction has we are provided with, and we get the following.

![2023-08-06 17_13_37-Sepolia Transaction Hash (Txhash) Details _ Etherscan — Mozilla Firefox.png]({{site.baseurl}}/assets/2023-08-06 17_13_37-Sepolia Transaction Hash (Txhash) Details _ Etherscan — Mozilla Firefox.png)

Let's take a step back to know where are heading, we are tasked with tracing this transaction to get the first address sent money to the receiver's wallet. Sounds easy :'D

By simply clicking on the address, which received the coin, we get all the transactions that occured from and to that wallet. 

![2023-08-06 17_16_59-Sepolia Transactions Information _ Etherscan — Mozilla Firefox.png]({{site.baseurl}}/assets/2023-08-06 17_16_59-Sepolia Transactions Information _ Etherscan — Mozilla Firefox.png)

We scroll alllllllllllll the way down and we find that first transaction we are tracing.
![2023-08-06 17_18_12-Sepolia Transactions Information _ Etherscan — Mozilla Firefox.png]({{site.baseurl}}/assets/2023-08-06 17_18_12-Sepolia Transactions Information _ Etherscan — Mozilla Firefox.png)

Clicking on that transaction, we find the address which sent 0.5ETH.

![2023-08-06 17_19_09-Sepolia Transaction Hash (Txhash) Details _ Etherscan — Mozilla Firefox.png]({{site.baseurl}}/assets/2023-08-06 17_19_09-Sepolia Transaction Hash (Txhash) Details _ Etherscan — Mozilla Firefox.png)

Flag:```ASCWG{0xEDaf4083F29753753d0Cd6c3C50ACEb08c87b5BD}```

## Fourth Challenge: We have a new contract for you

Description:```During further investigation on the wallet that received money that we found from the transaction hash in the previous challenge, we noticed there's a token that was generated, and used to send money as part of money laundring. Could find the last address that received tokens from this contract, and the address of the smart contract?```

Flag Format:```ASCWG{contract address: last wallet address}```

Okay, this is a LIL bit tricky. In this challenge we are presented with a smart contract which helped in money laundring, and we are tasked with getting the address of that contract and the last address which received any money from it.

Let's first examine the transactions from the previous challenge and see the smart contract transactions.

If we looked just above the first transaction shown before, we can see there's a contract creation.
![2023-08-06 23_00_49-Sepolia Transactions Information _ Etherscan — Mozilla Firefox.png]({{site.baseurl}}/assets/2023-08-06 23_00_49-Sepolia Transactions Information _ Etherscan — Mozilla Firefox.png)

By clicking on contract creation, we get the first part of the flag, which is the contract's address.

![2023-08-06 23_03_33-Contract Address 0xccc5eabd8f858f7a59f513875ba8ac12b9ac03d7 _ Etherscan — Mozill.png]({{site.baseurl}}/assets/2023-08-06 23_03_33-Contract Address 0xccc5eabd8f858f7a59f513875ba8ac12b9ac03d7 _ Etherscan — Mozill.png)

Contract address:```0xcCC5eaBD8F858f7A59f513875Ba8AC12b9aC03D7```

Now let's get back to the transactions page and see the rest of them. We notice there are 3 transactions.

![2023-08-06 23_17_40-ERC1967Proxy _ Address 0x7b799dfbbfd91eca3e82f2378455ab09463f1c73 _ Etherscan — .png]({{site.baseurl}}/assets/2023-08-06 23_17_40-ERC1967Proxy _ Address 0x7b799dfbbfd91eca3e82f2378455ab09463f1c73 _ Etherscan — .png)

Here comes the tricky part, let's investigate the last transaction that occured with this contract, which occured from an address that ends with ```be0e0``` we find a lot of transactions both out and in, but we only need the last sending address, which is ending with ```B97```.

![2023-08-06 23_21_00-Sepolia Transactions Information _ Etherscan — Mozilla Firefox.png]({{site.baseurl}}/assets/2023-08-06 23_21_00-Sepolia Transactions Information _ Etherscan — Mozilla Firefox.png)

And we get the next part of the flag.

Flag:```ASCWG{0x7b799DFbbfD91ECA3E82F2378455ab09463f1C73:0x3a79022e90fF621cd3Cc54FE95873E1A50722B97}```

## Fifth Challenge: Baby, I'm a killer.

Description:```A hacker managed to install and deploy another kernel driver in the same machine from which we received the memory dump in the first two challenges, Could you find when it was first installed and its first key value?```

NOTE: you need to download this memdump, as it's entirely different from the one in the first two challenges.

Difficulty:```Hard```

Flag format:```ASCWG{YYYY-MM-DD-HH:MM:SS-(timezone)- full key value}```

Okay, this is another memory dump, which also has a kernel driver, that was installed in system32 folder and blended in with the other drivers. Let's first see it.

Using out favourite tool volatility3, we use a new plugin called ```windows.modules.Modules```, and wait for it to list all the drivers.

![2023-08-06 23_35_56-Ubuntu22.04 - VMware Workstation.png]({{site.baseurl}}/assets/2023-08-06 23_35_56-Ubuntu22.04 - VMware Workstation.png)

At the end of the list we notice a driver called ```pplkiller.sys```, and the challenge's name has the word killer in it, so it might be it, OR if we prefer the long road. We could simply google it :D

![2023-08-06 23_36_51-Ubuntu22.04 - VMware Workstation.png]({{site.baseurl}}/assets/2023-08-06 23_36_51-Ubuntu22.04 - VMware Workstation.png)

Okay, so now what? What we need now, volatility won't be able to provide us with. We need to use another tool called [MemProcFS](https://github.com/ufrisk/MemProcFS).

the huge advantage of MemProcFS that it helps to navigate the memory dump any normal drive via the GUI instead of relying on commands and waiting for the output. It mounts the memdump as a mount share.

![2023-08-06 23_45_37-Windows 10 x64 - VMware Workstation.png]({{site.baseurl}}/assets/2023-08-06 23_45_37-Windows 10 x64 - VMware Workstation.png)

Quick Note:You can run it either from CMD or powershell, but I just prefer powershell.

Okay, now we naviagte to the driver in question which is named pplkiller.sys

![2023-08-06 23_47_41-Windows 10 x64 - VMware Workstation.png]({{site.baseurl}}/assets/2023-08-06 23_47_41-Windows 10 x64 - VMware Workstation.png)


![2023-08-06 23_49_33-Windows 10 x64 - VMware Workstation.png]({{site.baseurl}}/assets/2023-08-06 23_49_33-Windows 10 x64 - VMware Workstation.png)


After looking at the driver's files, we found nothing useful, so let's check the service folder, and we navigate to the folder which has the same driver's name i.e ```pplkiller.sys```

![2023-08-06 23_53_43-Windows 10 x64 - VMware Workstation.png]({{site.baseurl}}/assets/2023-08-06 23_53_43-Windows 10 x64 - VMware Workstation.png)

Opening the first file with the name ```(_key_.txt)```, we get the flag.

Flag:```ASCWG{2023-07-24-10:44:36-UTC-ffffde09bdc56000:003bfd28}```

This concludes this year's challenges I wanted this year to be different than last year, and I hope the players enjoyed solving the challenges and learned something new in the process.

Congratulations to the winning teams, and I hope to see you all in the final round.
