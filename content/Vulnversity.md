---
title: Vulnversity CTF Writeup
tags:
  - ctf
  - easy
  - tryhackme
  - recon
  - privilege_escalation
  - web
---
# [Vulnversity CTF](https://tryhackme.com/r/room/vulnversity) - TryHackMe

**Author:** [tryhackme](https://tryhackme.com/p/tryhackme),[SecurityNomad](https://tryhackme.com/p/SecurityNomad),[1337rce](https://tryhackme.com/p/1337rce)
**Date:** 2020-03-21
**Category:** General   
**Difficulty:** Easy   

---
## Challenge Description

Learn about active recon, web app attacks and privilege escalation.
The CTF is divided into 5 tasks
-  [Reconnaissance](#reconnaissance)
- Locating Directories using [[Gobuster]]
- Compromise the webserver
- Privilege Escalation

---

### Reconnaissance

Scan the given host via [[Nmap]] . Ping the host first. There is no response so [[ICMP]] packets maybe blocked. So we have to scan with ping off.
```
nmap <ip-address> -pN -sV -T4 -vv
```

The result gives pretty clear answers of all the questions in task 1.

### GoBuster

After that on the webserver port run goBuster with the provided wordlist mentioned in the CTF and get the name of a directory on the webserver
```
gobuster dir -u http://10.10.66.169:3333 -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt
```

I will complete this write up soon