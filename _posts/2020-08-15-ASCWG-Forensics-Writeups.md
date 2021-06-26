# The Site Settings
# v2.0
# https://github.com/cotes2020/jekyll-theme-chirpy
# © 2017-2019 Cotes Chung
# MIT licensed


# jekyll-seo-tag settings › https://github.com/jekyll/jekyll-seo-tag/blob/master/docs/usage.md
#--------------------------

title: infosec-blog                          # the main title

tagline: 0x01g33k blog  # it will display as the sub-title

description: >-                        # used by seo meta and the atom feed
  This is my blog for security stuff for 1337 HAX0R5.

# Replace with the website url, e.g. 'https://username.github.io'
url: 'https://0x01g33k.github.io/'

author: 0x01g33k                  # change to your full name

avatar: /assets/img/sample/logoo.png   # support internet resources

github:
  username: 0x01g33k             # change to your github username

twitter:
  username: 0x01g33k            # change to your twitter username

social:
  name: 0x01g33k                  # it will shows as the copyright owner in Footer
  #email: example@doamin.com             # change to your email address
  links:
    # The first element serves as the copyright owner's link
    - https://twitter.com/0x01g33k      # change to your twitter homepage
    - https://github.com/0x01g33k       # change to your github homepage
    # Uncomment below to add more social links
    # - https://www.facebook.com/username
    # - https://www.linkedin.com/in/username

#google_site_verification: google_meta_tag_verification # change to your verification string

#--------------------------


# Only if your site type is GitHub Project sites and doesn't have a custom domain,
# change below value to '/projectname'.
baseurl: 'infosec-blog'

# Change to your timezone › http://www.timezoneconverter.com/cgi-bin/findzone/findzone
timezone: Africa/Cairo

google_analytics:
  id: ''          # Fill with your Google Analytics ID
  pv:
    # The Google Analytics pageviews switch.
    # DO NOT enable it unless you know how to deploy the Google Analytics superProxy.
    enabled: false
    # the next options only valid when `google_analytics.pv` is enabled.
    proxy_url: ''
    proxy_endpoint: ''
    cache: false  # pv data local cache, good for the users from GFW area.

disqus:
  comments: false  # boolean type, the global switch for posts comments.
  shortname: ''    # Fill with your Disqus shortname. › https://help.disqus.com/en/articles/1717111-what-s-a-shortname


# Prefer color scheme setting, available values:
#
#     dual   - Follow the system prefer color by default, and a toggle will display
#              in the left bottom of Sidebar, which used for switch the theme between dark and light.
#
#     light  - Use the light color scheme
#
#     dark   - Use the dark color scheme
#
theme_mode: dark

# boolean type, global switch for ToC in posts.
toc: true

paginate: 10

markdown: kramdown

highlighter: rouge

kramdown:
  input: GFM
  syntax_highlighter: rouge
  syntax_highlighter_opts: # Rouge Options › https://github.com/jneen/rouge#full-options
    css_class: 'highlight'
    # default_lang: console
    span:
      line_numbers: false
    block:
      line_numbers: true
      start_line: 1

# DO NOT change this unless you're a Pro user on Jekyll and Web development,
# or you think you're smart enough to change other relevant URLs within this template.
permalink: /posts/:title/

defaults:
  -
    scope:
      path: ""          # An empty string here means all files in the project
      type: posts
    values:
      layout: post
      comments: true    # Enable comments in posts.
      toc: true         # Display TOC column in posts.
      location: Post
      breadcrumb:
        -
          label: Posts
          url: /
  -
    scope:
      path: _drafts
    values:
      comments: false
  -
    scope:
      path: tags
    values:
      tab_active: Tags
      location: Tag
      breadcrumb:
        -
          label: Home
          url: /infosec-blog
        -
          label: Tags
          url: /tabs/tags/
  -
    scope:
      path: categories
    values:
      tab_active: Categories
      location: Category
      breadcrumb:
        -
          label: Home
          url: /infosec-blog
        -
          label: Categories
          url: /tabs/categories/
  -
    scope:
      path: tabs
    values:
      layout: page
      dynamic_title: true    # Hide title in mobile screens.
      breadcrumb:
        -
          label: Home
          url: /infosec-blog


sass:
  sass_dir: /assets/css
  style: compressed

compress_html:
  clippings: all
  comments: ["<!-- ", " -->"]
  endings: [html, head, body, dt, dd, rt, rp,
            optgroup, option, colgroup, caption,
            thead, tbody, tfoot, tr, td, th]
  profile: false
  blanklines: false
  ignore:
    envs: []

exclude:
  - vendor        # Avoid Jekyll mistakenly read the vendor directory on Travis-CI's VM .
  - Gemfile.lock
  - Gemfile
  - tools
  - docs
  - README.md
  - LICENSE

sitemap_exclude:  # Sitemap will exclude the following items.
  fuzzy:
    - /assets/
  accurate:
    - /norobots/
    - /tabs/
    - /categories/
    - /tags/
    - /posts/
    - 404.html
    - feed.xml
    - sitemap.xml
    - robots.txt
    - redirects.json
g33k@0x01g33k:~/infosec-blog$ 
g33k@0x01g33k:~/infosec-blog$ 
g33k@0x01g33k:~/infosec-blog$ cd _posts/
g33k@0x01g33k:~/infosec-blog/_posts$ path
Command 'path' not found, did you mean:
  command 'xpath' from deb libxml-xpath-perl
  command 'pat' from deb dist
  command 'path6' from deb ipv6-toolkit
  command 'path6' from deb ipv6toolkit
  command 'mpath' from deb libmodule-path-perl
  command 'patch' from deb patch
Try: sudo apt install <deb name>
g33k@0x01g33k:~/infosec-blog/_posts$ pwd
/home/g33k/infosec-blog/_posts
g33k@0x01g33k:~/infosec-blog/_posts$ ls
2020-08-15-ASCWG-Forensics-Writeups.md
g33k@0x01g33k:~/infosec-blog/_posts$ cat 
^C
g33k@0x01g33k:~/infosec-blog/_posts$ cat 2020-08-15-ASCWG-Forensics-Writeups.md 
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
