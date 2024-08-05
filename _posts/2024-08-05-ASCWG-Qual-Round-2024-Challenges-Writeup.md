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




