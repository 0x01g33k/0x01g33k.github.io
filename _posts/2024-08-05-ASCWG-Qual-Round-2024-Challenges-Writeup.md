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

Flag:```ASCWG{192.168.159.128:502-WaterTank-8000```




