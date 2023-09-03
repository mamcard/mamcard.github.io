---
title: "HTB: CozyHosting"
date: 2023-09-03T12:17:59+01:00
draft: false
---

# Recon
## nmap
We start with the regular `nmap` scan, and we found two TCP ports open, SSH (22) and HTTP (80)

`
[miguel@druid cozyhosting]$ nmap -p- --min-rate 1000 10.10.11.230
Starting Nmap 7.94 ( https://nmap.org ) at 2023-09-03 12:24 WEST
Nmap scan report for 10.10.11.230
Host is up (0.062s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 129.26 seconds
[miguel@druid cozyhosting]$ nmap -p 22,80 -sCV 10.10.11.230
Starting Nmap 7.94 ( https://nmap.org ) at 2023-09-03 12:27 WEST
Nmap scan report for 10.10.11.230
Host is up (0.078s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 43:56:bc:a7:f2:ec:46:dd:c1:0f:83:30:4c:2c:aa:a8 (ECDSA)
|_  256 6f:7a:6c:3f:a6:8d:e2:75:95:d4:7b:71:ac:4f:7e:42 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://cozyhosting.htb
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.74 seconds
`

From the [OpenSSH](https://packages.ubuntu.com/search?suite=default&section=all&arch=any&keywords=openssh-server&searchon=names) version we can see that the host is likely running Ubuntu Jammy 22.04 LTS.


## Website - TCP Port 80
Time to look at the page itself. Seems like a Hosting service. At first, the only version number that pops up is `Bootstrap v5.2.3`, but looking around for vulnerabilities seems fruitless. We do find a login page at `cozyhosting.htb/login`, this could be useful.

