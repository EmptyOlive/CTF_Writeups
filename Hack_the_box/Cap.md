# Attack IP
  10.10.10.245

#Difficulty
  Community rating - Easy

#Category 
"""
Cap is an easy difficulty Linux machine running an HTTP server that performs administrative functions including performing network captures.
Improper controls result in Insecure Direct Object Reference (IDOR) giving access to another user's capture.
The capture contains plaintext credentials and can be used to gain foothold. A Linux capability is then leveraged to escalate to root.
"""

Method of Solve

Nmap scan 
""""
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 fa:80:a9:b2:ca:3b:88:69:a4:28:9e:39:0d:27:d5:75 (RSA)
|   256 96:d8:f8:e3:e8:f7:71:36:c5:49:d5:9d:b6:a4:c9:0c (ECDSA)
|_  256 3f:d0:ff:91:eb:3b:f6:e1:9f:2e:8d:de:b3:de:b2:18 (ED25519)
80/tcp open  http    Gunicorn
|_http-title: Security Dashboard
|_http-server-header: gunicorn
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
"""
Task 1: How many TCP ports are open?
Answer: 3

Heading to website we see the security dashboard, using the guide from the second task we head to Security Snapshot

Task 2: After running a "Security Snapshot", the browser is redirected to a path of the format /[something]/[id], where [id] represents the id number of the scan. What is the [something]?
Answer: data

from here using the question as a guide again we change the number after data/ to see if we can view other peoples scans.

Task 3: Are you able to get to other users' scans?
Answer: Yes
