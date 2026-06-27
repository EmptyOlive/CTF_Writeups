# Box Name 
Silentium

# Community Rating
Easy

# NMAP Scan

```
sudo nmap -p- -sC -sV -O 10.129.101.220 --min-rate 5000
sudo: unable to resolve host kali: Name or service not known
[sudo] password for kali: 
Starting Nmap 7.95 ( https://nmap.org ) at 2026-06-08 17:49 EDT
Warning: 10.129.101.220 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.129.101.220
Host is up (0.70s latency).
Not shown: 37951 closed tcp ports (reset), 27582 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.15 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
|_  256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
80/tcp open  http    nginx 1.24.0 (Ubuntu)
|_http-server-header: nginx/1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to http://silentium.htb/
Device type: general purpose
Running: Linux 5.X
OS CPE: cpe:/o:linux:linux_kernel:5
OS details: Linux 5.0 - 5.14
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 152.42 seconds
```

# Method of solve 

First we go the website. see no available paths.

then we run a go buster 
```
gobuster vhost -u http://silentium.htb -w /usr/share/wordlists/dirb/common.txt --append-domain
```
we find staging.silentium
after adding to hosts we see a login screen. 
Searching for known vulnerabilities we find CVE-2025-58434 
using to exploit the server 
