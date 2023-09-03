---
title: "THM: Expose"
date: 2023-09-03T17:17:25+01:00
draft: false
---


# THM: Expose

| Name | Expose |
| ----------- | ----------- |
| Header | Title |
| Paragraph | Text |

---

# Recon

## nmap
We found port 21 (ftp), 22 (ssh), 53 (dns), 1337 (http) and 1883 (mosquitto) open.

```bash
[miguel@druid expose]$ nmap -p- --min-rate 1000 10.10.48.38
Starting Nmap 7.94 ( https://nmap.org ) at 2023-09-03 17:01 WEST
Nmap scan report for 10.10.48.38
Host is up (0.067s latency).
Not shown: 65530 closed tcp ports (conn-refused)
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
53/tcp   open  domain
1337/tcp open  waste
1883/tcp open  mqtt

Nmap done: 1 IP address (1 host up) scanned in 45.64 seconds

[miguel@druid expose]$ nmap -p 21,22,53,1337,1883 -sCV 10.10.48.38
Starting Nmap 7.94 ( https://nmap.org ) at 2023-09-03 17:02 WEST
Nmap scan report for 10.10.48.38
Host is up (0.070s latency).

PORT     STATE SERVICE                 VERSION
21/tcp   open  ftp                     vsftpd 2.0.8 or later
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst:
|   STAT:
| FTP server status:
|      Connected to ::ffff:10.14.34.147
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp   open  ssh                     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 7a:8c:8b:24:16:b1:d0:27:61:9d:57:0c:4b:e7:19:4f (RSA)
|   256 6a:96:c5:3e:11:aa:6f:36:c9:84:f9:81:6f:d2:ea:c4 (ECDSA)
|_  256 84:a2:66:70:5f:53:ec:88:f4:a1:59:51:56:70:31:28 (ED25519)
53/tcp   open  domain                  ISC BIND 9.16.1 (Ubuntu Linux)
| dns-nsid:
|_  bind.version: 9.16.1-Ubuntu
1337/tcp open  http                    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: EXPOSED
1883/tcp open  mosquitto version 1.6.9
| mqtt-subscribe:
|   Topics and their most recent payloads:
|     $SYS/broker/load/bytes/received/5min: 3.53
|     $SYS/broker/bytes/received: 18
|     $SYS/broker/load/connections/5min: 0.20
|     $SYS/broker/messages/received: 1
|     $SYS/broker/version: mosquitto version 1.6.9
|     $SYS/broker/load/bytes/sent/5min: 0.79
|     $SYS/broker/load/connections/1min: 0.91
|     $SYS/broker/uptime: 264 seconds
|     $SYS/broker/load/messages/received/15min: 0.07
|     $SYS/broker/store/messages/bytes: 179
|     $SYS/broker/load/bytes/received/15min: 1.19
|     $SYS/broker/bytes/sent: 4
|     $SYS/broker/load/messages/sent/1min: 0.91
|     $SYS/broker/messages/sent: 1
|     $SYS/broker/load/bytes/received/1min: 16.45
|     $SYS/broker/load/sockets/1min: 2.08
|     $SYS/broker/load/sockets/5min: 0.54
|     $SYS/broker/load/bytes/sent/1min: 3.65
|     $SYS/broker/load/messages/sent/5min: 0.20
|     $SYS/broker/load/messages/sent/15min: 0.07
|     $SYS/broker/load/bytes/sent/15min: 0.27
|     $SYS/broker/load/connections/15min: 0.07
|     $SYS/broker/load/sockets/15min: 0.19
|     $SYS/broker/load/messages/received/5min: 0.20
|     $SYS/broker/heap/maximum: 49688
|_    $SYS/broker/load/messages/received/1min: 0.91
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## Port 21 - ftp
We are able to access ftp with the Anonymous user without providing any password. We confirmed this, but saw that there was nothing there. Let's leave this for now and check another service.

## Port 1337 - http
We see a very simple website
![alt text](port1337.jpg)
