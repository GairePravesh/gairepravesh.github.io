---
title: "Advent of Cyber 2"
categories:
 - tryhackme
tags:
 - easy
 - writeups
author_profile: true
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

#### [Day 4] <span style="color:red">**Web Exploitation**</span> - Santa's Watching

#### [Day 5] <span style="color:red">**Web Exploitation**</span> - Someone stole Santa's gift list!

#### [Day 6] <span style="color:red">**Web Exploitation**</span> - Be careful with what you wish on Christmas night!

#### [Day 7] <span style="color:#2ee7b6">**Special by John Hammond**</span> - Coal for Christmas

#### [Day 8] <span style="color:orange">**Networking**</span> - The Grinch Really Did Steal Christmas

#### [Day 9] <span style="color:orange">**Networking**</span> - What's Under the Christmas Tree? 

#### [Day 10] <span style="color:orange">**Networking**</span> - Anyone can be Santa! 

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



