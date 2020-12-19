---
title: "Advent of Cyber 2"
categories:
 - tryhackme
tags:
 - easy
 - writeups
author_profile: true
published: false
---

*Get started with Cyber Security in 25 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.*

[Room](https://tryhackme.com/room/adventofcyber2)

#### [Day 1] <span style="color:red">**Web Exploitation**</span> - A Christmas Crisis

> **What is the name of the cookie used for authentication ?**

```
auth
```

> **In what format is the value of this cookie encoded ?**

```
JSON
```

> **Figure out how to bypass the authentication. <br>
  What is the value of Santa's cookie?**

The cookie was in json form with a field for username, 
replace it with `santa` and hex-encode again to get the auth cookie of Santa

```
7b22636f6c20436.........222c2022757365726e616d652
```

> **What is the flag you're given when line is fully active ?**

Replace the cookie value, refresh and enable all the buttons

**Flag**: `THM{.......M1NjBmZWFhYmQy}`

#### [Day 2] <span style="color:red">**Web Exploitation**</span> - The ELF Strikes Back!

> **What string of text needs adding to the URL to get access to the upload page?**

```
?id=ODIzODI5MTNiYmYw
```

> What type of file is accepted by the site?

view-source has a list of acceptable ext.

```
image
```

> **Bypass the filter and upload a reverse shell.<br>
In which directory are the uploaded files stored?**

Since only extension check is done, write a php-reverse-shell and rename it to .jpg.php<br>
Now listen on a port for the connection

```
/uploads/
```

> **What is the flag in /var/www/flag.txt?**

**Flag**: `THM{MGU3Y2U..........OWJhMzhh}`


#### [Day 3] <span style="color:red">**Web Exploitation**</span> - Christmas Chaos

Using BurpSuite Intruder Sniper bruteforce the login credentials.

> **What is the flag?**

**Flag**: `THM{...........6f9d8fe99ad1a}`

#### [Day 4] <span style="color:red">**Web Exploitation**</span> - Santa's Watching

Using wfuzz for fuzzing parameters and gobuster for discovery scanning. 

> **Given the URL "http://shibes.xyz/api.php", what would the entire wfuzz command look like to query the "breed" parameter using the wordlist "big.txt" (assume that "big.txt" is in your current directory)**

```
wfuzz -c -z file,big.txt http://shibes.xyz/api.php?breed=FUZZ
```

> **Use GoBuster (against the target you deployed -- not the shibes.xyz domain) to find the API directory. What file is there?**

`gobuster dir -u http://$IP -w /usr/share/wordlist/big.txt`

```
site-log.php
```

> **Fuzz the date parameter on the file you found in the API directory. What is the flag displayed in the correct post?**

`wfuzz -c -z file,wordlist http://$IP/api/site-log.php -d "date=FUZZ"`

```
THM{..P1}
```

#### [Day 5] <span style="color:red">**Web Exploitation**</span> - Someone stole Santa's gift list!

> **Without using directory brute forcing, what's Santa's secret login panel?**

```
/santapanel
```

> **How many entries are there in the gift database?**

Put `' --` and all entries are given

```
22
```

> **What did Paul ask for?**

```
github ownership
```

> **What is the flag?**

Using burpsuite to save the search request and run sqlmap on it.

`sqlmap -r /root/Desktop/sqli2 --dbms=sqlite --dump-all --tamper=space2comment`

```
thmfox{All....._Christmas_Is_...}
```

> **What is admin's password?**

Inject `' union username, password from users --` to get the admin password

```
EhC......sc7gB
```

#### [Day 6] <span style="color:red">**Web Exploitation**</span> - Be careful with what you wish on Christmas night!

> **What vulnerability type was used to exploit the application?**

```
Stored cross-site scripting
```

> **What query string can be abused to craft a reflected XSS?**

```
q
```

> **Run a ZAP (zaproxy) automated scan on the target. How many XSS alerts are in the scan?**

```
2
```

#### [Day 7] <span style="color:#2ee7b6">**Special by John Hammond**</span> - Coal for Christmas

> **Open "pcap1.pcap" in Wireshark. What is the IP address that initiates an ICMP/ping?**

```
10.11.3.2
```

> **If we only wanted to see HTTP GET requests in our "pcap1.pcap" file, what filter would we use?**

```
http.request.method == get
```

> **Now apply this filter to "pcap1.pcap" in Wireshark, what is the name of the article that the IP address "10.10.67.199" visited?**

```
reindeer-of-the-week
```

> **Let's begin analysing "pcap2.pcap". Look at the captured FTP traffic; what password was leaked during the login process?There's a lot of irrelevant data here - Using a filter here would be useful!**

```
plaintext_password_fiasco
```

> **Continuing with our analysis of "pcap2.pcap", what is the name of the protocol that is encrypted?**

```
ssh
```

> **Analyse "pcap3.pcap" and recover Christmas!What is on Elf McSkidy's wishlist that will be used to replace Elf McEager?**

Export the http

```
rubber ducky
```

#### [Day 8] <span style="color:orange">**Networking**</span> - The Grinch Really Did Steal Christmas

> **When was Snort created?**

```
1998
```

> **Using Nmap on 10.10.49.37 , what are the port numbers of the three services running?**

`nmap -p- -v --max-retries 0 $IP -Pn`

```
80, 2222, 3389
```

> **Use Nmap to determine the name of the Linux distribution that is running, what is reported as the most likely distribution to be running?**

```
sudo nmap -O $IP
```

> **Use Nmap's Network Scripting Engine (NSE) to retrieve the "HTTP-TITLE" of the webserver. Based on the value returned, what do we think this website might be used for?**

```
Blog
```

#### [Day 9] <span style="color:orange">**Networking**</span> - What's Under the Christmas Tree? 

> **Question #1: Name the directory on the FTP server that has data accessible by the "anonymous" user**

`ftp $IP` with creds `anonymous:anonymous`

```
public
```

> **What script gets executed within this directory?**

```
backup.sh
```

> **What movie did Santa have on his Christmas shopping list?**

Download the file using `get` command and read

```
The Polar Express
```

> **Re-upload this script to contain malicious data; Output the contents of /root/flag.txt!**

Put a reverse shell script in `backup.sh` and listen on a port to get the shell

```
THM{even_.........._santa}
```

#### [Day 10] <span style="color:orange">**Networking**</span> - Anyone can be Santa! 

> **Using enum4linux, how many users are there on the Samba server (10.10.49.37)?**

`enum4linux $IP`

```
3
```

> **Now how many "shares" are there on the Samba server?**

`smbclient -L \\\\$IP

```
4
```

> **Use smbclient to try to login to the shares on the Samba server (10.10.49.37). What share doesn't require a password?**

`smbclient \\\\$IP\\tbfc-santa`

```
tbfc-santa
```

> **Log in to this share, what directory did ElfMcSkidy leave for Santa?**

```
jingle-tunes
```

#### [Day 11] <span style="color:orange">**Networking**</span> - Don't be Elfish! 



#### [Day 12] <span style="color:orange">**Networking**</span> - The Rogue Gnome 

#### [Day 13] <span style="color:orange">**Networking**</span> - Ready, set, elf. 

#### [Day 14] <span style="color:#2ee7b6">**Special by TheCyberMentor**</span> - Where's Rudolph? 

#### [Day 15] <span style="color:grey">**Scripting**</span> - There's a Python in my stocking! 

#### [Day 16] <span style="color:grey">**Scripting**</span> - Help! Where is Santa? 

#### [Day 17] <span style="color:purple">**Reverse Engineering**</span> - ReverseELFneering

#### [Day 18] <span style="color:purple">**Reverse Engineering**</span> - The Bits of the Christmas 

#### [Day 19] <span style="color:#2ee7b6">**Special by Tib3rius**</span> 

#### [Day 20] <span style="color:blue">**Blue Teaming**</span> - PowershELlF to the rescue 

#### [Day 21] <span style="color:blue">**Blue Teaming**</span> - Time for some ELForensics 

#### [Day 22] <span style="color:blue">**Blue Teaming**</span> - Elf McEager becomes CyberElf

#### [Day 23] <span style="color:blue">**Blue Teaming**</span> - The Grinch strikes again! 

#### [Day 24] <span style="color:#2ee7b6">**Special by DarkStar**</span> - The Trial Before Christmas

#### [Day 25] Christmas Day



