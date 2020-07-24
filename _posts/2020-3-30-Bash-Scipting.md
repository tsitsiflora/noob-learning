---
layout: post
title: Automation with bash!
---

## What is Shell/Bash Scripting

![]({{ site.baseurl }}/images/post1.jpg)

- "Shell" is a program, which facilitates the interaction between the user and operating system (kernel). There are many shells available, like sh, bash, csh, zsh...etc.
- "Shell" scripting is a way of automating things, in the form of collection of commands. The control of execution is steered by the predefined control statements.
- "Bash shell" scripting is, a kind of shell scripting only. You can say, its a subset of "shell" scripting.
- "sh" is the original (Bourne) shell, having its root from the old Unix. "bash", is "Bourne Again SHell", which is rewritten "sh".
- Bash is the most widely used shell. It comes with Linux by default, having backward compatibility with sh (though sh is also there). But, you can choose any shell you want.
- For more information, do:
$~ man bash

Basically, bash scripting is writing cli commands in a file and executing them the same way one would execute a program written in a high level language, such as python. Most system administrators use bash to automate tasks and make their job easier by not repeating common jobs everyday.

Okay, enough with the explanations, let's get into it.


## Simple Bash Program

In this program we want to create a directory and in that directory, create a new file and write some stuff in the new file. In the command line this would be 3 lines of commands, so it's as easy as that as well.

I'll assume we are in the Desktop directory:

```
nano firstscript
```
and write the following:


```
#!/bin/bash

mkdir NewFolder && cd NewFolder
touch newfile
echo "I just created this new file and I have nothing to write in it" > newfile
```
#!/bin/bash: this is known as the shebang. It tells the interpreter that the following lines are written for bash, so execute this file as a bash script.

Bash scripts are usually executed like so: ./scriptname

So the shebang determines how to interpret the script, sometimes it could be any of the following:

```
#!/bin/sh
#!/usr/bin/perl
#!/usr/bin/python
#!/bin/bash
#!/bin/ksh
```

In order to make our script executable, we have to add the executable mode to the file. We'll do this using the following command:

```
chmod +x firstScript
```

After executing the script, a new folder should be created on the Desktop, with a file in it that says whatever you wrote it should say

## Another fun example, not really

In the following script we are going to take input from the user, use base64 to encrypt that input and print the result to a file

```
#!/bin/bash

echo "Here is your result" > result
echo -n "$@" | base64 >> result
```

The above example just shows what we did in the first example, the only difference here is we're using >> to append to a file.

To provide a string that will be encoded, you have to provide the string when you execute the script. This is what's represented by the $@ sign, the string you provide will be the one that will be encoded using base64.

e.g ./test ThisIsMyPassword

Have fun!


