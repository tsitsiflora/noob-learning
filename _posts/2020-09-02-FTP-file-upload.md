---
layout: post
title: Python Script to uload file to an FTP server
---

## Boring Explanations *tldr

I was working on **HTB** the other day and came across a mini hurdle that led to this post. I needed to upload a **PHP reverse shell** via **ftp**, and I had already acquired the credentials to login via ftp. However, ftp was operating file upload in **binary mode**, hence nomatter how many times i tried to upload the file using **put** , it did not work.

![FTP Binary Mode]({{site.url }}/imgs/ftp/ftp1.png)

So i did some research and figured I can write a python script with less than 10 lines to do the job. 

- HTB: Hack the Box(htb) is an online platform with over 200 vulnerable machines that can be manipulated to practice and develop penetration testing skills. It is a really great way to learn about real life vulnerabilities as well as enumeration and privelege escalation techniques. View their site [here](https://www.hackthebox.eu).
- A reverse shell: A reverse shell is a shell session established on a connection that is initiated from a remote machine, not from the local host. Attackers who successfully exploit a remote command execution vulnerability can use a reverse shell to obtain an interactive shell session on the target machine and continue their attack.You can read more about reverse shells [here](https://www.netsparker.com/blog/web-security/understanding-reverse-shells/).
- FTP: File Transfer Protocol:The File Transfer Protocol (FTP) is a standard network protocol used for the transfer of computer files between a client and server on a computer network. FTP is built on a client-server model architecture using separate control and data connections between the client and the server.Read more about ftp [here](https://searchnetworking.techtarget.com/definition/File-Transfer-Protocol-FTP).
- FTP binary mode: Binary transfer mode is an FTP mode used to transfer files without modification or conversion. Files are transferred without conversion resulting in the same file on the source computer as the destination computer. Visit this [link](https://knowledge.broadcom.com/external/article/28212/ftp-ascii-vs-binary-mode-what-it-means.html) to understand more.
- FTP put:This is a command used to copy a file from the local system to the remote system. This [site](http://www.simotime.com/ftp4cmd1.htm#FTPcommandlist) shows a list of ftp commands and their descriptions.

## The script

![FTP Binary Mode]({{site.url }}/imgs/ftp/ftp2.png)

We'll be breaking down the script and explaining it line by line.

```
import ftplib
```
ftplib is an FTP protocol client that implements the client side of FTP protocol. We need to use this to connect to the ftp server hence we import it. 

```
server = ftplib.FTP('10.10.10.197', 'username', 'password')
```

Next, we define the ftp server(the url which we want to connect to) and the credentials we are going to be using to login. 

```
with open('revshell.php', 'rb') as upload_file:
    server.storbinary('STOR username/revshell.php', upload_file)
```

Next we make sure that the reverse shell that we want to upload to the ftp server is in the directory that we are working in. We open the file in binary mode(**rb**) and use the method storbinary to upload it. The method [storbinary()](https://pythontic.com/ftplib/ftp/storbinary) initiates a file transfer from an FTP client to a an FTP server using the FTP command STOR.
The file transfer is done in binary mode.

```
print(server.dir('username'))
```
Lastly, we issue a print statement to view the file we just uploaded in the user directory.

Here's the code in full:

```
import ftplib

server = ftplib.FTP('10.10.10.197', 'username', 'password')
with open('revshell.php', 'rb') as upload_file:
    server.storbinary('STOR username/revshell.php', upload_file)

print(server.dir('username'))

```

There you have it, a very easy and straight forward script that can come in handy!