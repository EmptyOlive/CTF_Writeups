# Synopsis

Nexus is an easy-difficulty Linux machine that features an exposed Gitea respository leaking credentials and a job posting that reveals valid usernames. 
The leaked credentials provide access to Krayin CRM, which is vulnerable to CVE-2026-38526, leading to a shell as www-data. 
Futher enumeration of the Krayin CRM configuration files reveals additional credentials that allow SSH access. 
Service enumeration reveals a Gitea template sync service vulnerable to directory traversal, which is leveraged to gain a shell as root.

# Enumeration
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
|_  256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
80/tcp open  http    nginx 1.24.0 (Ubuntu)
|_http-title: Nexus Energy Authority \xE2\x80\x94 Powering the Nation's Future
|_http-server-header: nginx/1.24.0 (Ubuntu)
Device type: general purpose|router
Running: Linux 5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 5.0 - 5.14, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 21/tcp)
HOP RTT       ADDRESS
1   467.23 ms 10.10.14.1
2   467.82 ms nexus.htb (10.129.234.54)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 3906.67 seconds
```
From here we use gobuster for subdomain enumeration
```
gobuster vhost -u http://nexus.htb -w /usr/share/wordlists/dirb/common.txt --append-domain
```
Using this we see some interesting subdomains
```
billing.nexus.htb Status: 302 [Size: 390] [--> http://billing.nexus.htb/admin/login]
cgi-bin/.nexus.htb Status: 400 [Size: 166]
CVS/Entries.nexus.htb Status: 400 [Size: 166]
CVS/Repository.nexus.htb Status: 400 [Size: 166]
CVS/Root.nexus.htb Status: 400 [Size: 166]
Documents and Settings.nexus.htb Status: 400 [Size: 166]
git.nexus.htb Status: 200 [Size: 14474]
```
going to git.nexus.htb and look around we find the docker compose file and the db username and password being N27xh!!2ucY04

from here we try the username and password to no avail. pivoting to billing subdomain, we try j.matthew@nexus.htb with password N27xh!!2ucY04 to give us access.







# Tasks

***Task 1***
How many open TCP ports are listening on Nexus?
Answer - 2

**Task 2**
What is the hiring manager's full email address?
Answer - j.matthew@nexus.htb

**Task 3**
What is the name of the additional subdomain hosting the Git service discovered during enumeration of nexus.htb
Answer - git

**Task 4**
What is the DB_PASSWORD discovered while enumerating the exposed repository?
Answer - N27xh!!2ucY04
