---
title: ASCWG Forensics Challenges Final Round writeup
published: true
---

Hello as it was an honor to write these challenges for the final round of Arab Security Cyber War Games, these are the writeups for said challenges.</p>


<p>Challenge 1:  Bamboozled</p>
![]({{site.baseurl}}/assets/bamboozled.png)
![bamboozled.png]({{site.baseurl}}/assets/bamboozled.png)

The challenge description says the hacker has hidden the sensitive files inside a picture. okay let's check this picture first.
![]({{site.baseurl}}/assets/2020-09-13-18_06_15-kali-linux-vmware-workstation.png)
![2020-09-13-18_06_15-kali-linux-vmware-workstation.png]({{site.baseurl}}/assets/2020-09-13-18_06_15-kali-linux-vmware-workstation.png)
From the looks of it, it doesn't have anything interesting so we need to go deeper, using exiftool we got something interesting.
![]({{site.baseurl}}/assets/2020-09-13-18_09_10-kali-linux-vmware-workstation.png)![2020-09-13-18_09_10-kali-linux-vmware-workstation.png]({{site.baseurl}}/assets/2020-09-13-18_09_10-kali-linux-vmware-workstation.png)

It looks like an md5 has, trying to crack it online using <a href="http://crackstation.net">crackstation.net</a> we get the password "alvium". Using steghide we were able to extract the Flag.txt file


<p> The flag " ASCWG{St3go_!s_funny_s0met!m3S}"</p>



<p>Challenge no.2: Disassembled</p>
![]({{site.baseurl}}/assets/disassambled.png)
![disassambled.png]({{site.baseurl}}/assets/disassambled.png)

So from the description we have an obfuscated file and we need to de-obufsucate it.

Looking at the downloaded rar file we get these weird pictures
![]({{site.baseurl}}/assets/2020-09-13-18_15_01-kali-linux-vmware-workstation.png)![2020-09-13-18_15_01-kali-linux-vmware-workstation.png]({{site.baseurl}}/assets/2020-09-13-18_15_01-kali-linux-vmware-workstation.png)

The directory has 1162 pictures and their names appear to be base64 encoded. We need to write a script to decode their names and then reassemble the pictures in the correct order. 

```
mkdir renamed_files

#Decode all of the image file names (base64)
for filename in `ls *.png`
do
    #Get the name of the file
    bn=`echo $filename | cut -d '.' -f 1`
    #Decode the name
    newName=`echo $bn | base64 -d`.png
    #Copy rename the file
    cp $filename renamed_files/$newName
done
echo "Successfuly decoded base 64 named files"

#Get max width and height by sorting each dimension and getting the max.
maxWidth=`ls renamed_files | cut -d '_' -f 1 | sort -nu | tail -n 1`
maxHeight=`ls renamed_files | cut -d '_' -f 2 | cut -d '.' -f 1 | sort -nu | tail -n 1`

printf "Max Width:\t%d\nMax Height:\t%d\n"  $maxWidth $maxHeight


#Construct the columns first
mkdir columns 

#Construct the following command to keep the files in order
#convert input1 input2 ... [-/+]append columns/_.png
#This MAY be limited by ARG_MAX in your system.

convertCommand=""
for i in `seq 0 $maxWidth`
do
convertCommand="convert "
    for j in `seq 0 $maxHeight`
    do
        convertCommand=$convertCommand" renamed_files/"$i"\_"$j".png "
    done
    convertCommand=$convertCommand" -append columns/$i.png"
    $convertCommand
    convertCommand="" #Reset the command string
    echo "Successfuly recreated columns/$i.png"
done

#Join the columns to construct the final image. 
convertCommand="convert "
for i in `seq 0 $maxWidth`
do
    convertCommand=$convertCommand"columns/"$i".png "
done

$convertCommand +append output.png
echo "Successfuly recreated the image at ./output.png"
```


Big thanks to my dear friend [Satharus](https://satharus.me/) for helping me write this script.
running the script inside the directory we get the final picture called output.png 
![]({{site.baseurl}}/assets/2020-09-13-18_33_43-kali-linux-vmware-workstation.png)![2020-09-13-18_33_43-kali-linux-vmware-workstation.png]({{site.baseurl}}/assets/2020-09-13-18_33_43-kali-linux-vmware-workstation.png)

The flag is : ASCWG{B@sH_scR!ptssSS}

<p>Challenge no.3: easy-peasy</p>
![]({{site.baseurl}}/assets/easy-peasy.png)![easy-peasy.png]({{site.baseurl}}/assets/easy-peasy.png)

The description hints that we may be facing a memory dump, running file command we get nothing useful. So we run volatility command with imageinfo plugin we get some interesting stuff.
![]({{site.baseurl}}/assets/2020-09-13-18_39_52-kali-linux-vmware-workstation.png)![2020-09-13-18_39_52-kali-linux-vmware-workstation.png]({{site.baseurl}}/assets/2020-09-13-18_39_52-kali-linux-vmware-workstation.png)

So it's a windows 7 memory dump, we need to dig deeper. using pslist plugin we are able to dump the processes that were running 
![]({{site.baseurl}}/assets/2020-09-13-18_42_40-kali-linux-vmware-workstation.png)![2020-09-13-18_42_40-kali-linux-vmware-workstation.png]({{site.baseurl}}/assets/2020-09-13-18_42_40-kali-linux-vmware-workstation.png)

We have two cmd.exe processes dumping their contents using consoles plugin, we get a mega drive link.
![]({{site.baseurl}}/assets/2020-09-13-18_44_37-kali-linux-vmware-workstation.png)![2020-09-13-18_44_37-kali-linux-vmware-workstation.png]({{site.baseurl}}/assets/2020-09-13-18_44_37-kali-linux-vmware-workstation.png)
going to that [link](https://mega.nz/file/ImQR1Coa#Dt3tr6Ze8Ibwconty_SEGq48N_xmkHiZAbyob8-TlSA) we get the flag.rar.rar file, cracking it using johntheripper. We get the password which is "princesa", we are able to get the flag. 
<p>ASCWG{M3moRy_f0Rens!Cs_ar3_FuN}</p>

<p>Challenge no.4: LookyLUKS</p>
![]({{site.baseurl}}/assets/lookyluks.png)
![lookyluks.png]({{site.baseurl}}/assets/lookyluks.png)

From the description we are facing an encrypted filesystem, and the name of the challenge hints that it's a LUKS filesystem. 

we need to use a tool to brute force the password, using a tool called <a href="https://github.com/glv2/bruteforce-luks">bruteforce-luks</a> . We are able to get the password for the challenge which is "xavier". mounting that drive using cryptsetup utility 

![]({{site.baseurl}}/assets/2020-09-13-18_53_05-kali-linux-vmware-workstation.png)![2020-09-13-18_53_05-kali-linux-vmware-workstation.png]({{site.baseurl}}/assets/2020-09-13-18_53_05-kali-linux-vmware-workstation.png)

where NULL is the name of the volume I want to mount. We are able to get the flag 

<p>ASCWG{Wh0_kn3w_LUKS_!s_cr@ckab1e}</p>


<p>Challenge no.5: Hydra 2</p>
This challenge was split into two parts Hydra 1 where the players need to extract the password for the rar file of Hydra 2, and then solve Hydra 2.

the password my friend <a href="https://satharus.me/cybersecurity/2020/09/13/ascwg2020_re.html">Satharus </a>wrote the writeup on how to reverse the binary and extract it, so please pay him a visit and see his writeup (he really did awesome work with writing the challenges)

The password is "the f1@g !s encrypt3d w!tH a l0g!c g@t3", now to work on Hydra 2.

The file appears to have some hex values judging from the first numbers that it's a rar file converted to hex. ![2020-09-13-19_02_36-kali-linux-vmware-workstation.png]({{site.baseurl}}/assets/2020-09-13-19_02_36-kali-linux-vmware-workstation.png)
we need to reverse these hex values to the original file, using xxd utility ![]({{site.baseurl}}/assets/2020-09-13-19_04_48-kali-linux-vmware-workstation.png)![2020-09-13-19_04_48-kali-linux-vmware-workstation.png]({{site.baseurl}}/assets/2020-09-13-19_04_48-kali-linux-vmware-workstation.png)

The Flag appears to be encrypted going back to the password its explicitly states that the flag is encrypted with a logic gate so it must be XORd, going to <a href="https://gchq.github.io/CyberChef/">cyberchef </a>to decrypt it, we get the flag.
<p>ASCWG{Wh0_kn3w_DF_and_R3_can_w0rk_t0g3tHeR}.</p>


And this concludes the forensics challenges for ASCWG it's been a wonderful experience that I hope to do it again.
