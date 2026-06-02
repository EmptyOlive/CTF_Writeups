# Box Name
  Reactor

# Community Rating
Easy


# Method of Solve
Nmap Scan
'''
PORT     STATE SERVICE VERSION                                                                                                                                                                                                              
22/tcp   open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)                                                                                                                                                       
| ssh-hostkey:                                                                                                                                                                                                                              
|   256 ce:fd:0d:82:c0:23:ed:6e:4b:ea:13:fa:4f:ea:ef:b7 (ECDSA)                                                                                                                                                                             
|_  256 f8:44:c6:46:58:7a:39:21:ef:16:44:e9:58:c2:f3:62 (ED25519)                                                                                                                                                                           
3000/tcp open  ppp?                                                                                                                                                                                                                         
| fingerprint-strings:                                                                                                                                                                                                                      
|   GetRequest:                                                                                                                                                                                                                             
|     HTTP/1.1 200 OK                                                                                                                                                                                                                       
...
                                                                                                                                                                                                                     
'''

Usernames found 
```
Dr. Elena Rodriguez
Lead Nuclear Engineer
Marcus Kim
Senior Technician
JT
James Thompson
Safety Officer
```
Nil produced by Fuff

using Wappalyzer, website is running next.js 15.0.3 googling we find a CVE to give us RCE
```
RCE in RSC Protocol (CVE-2025-66478 / CVE-2025-55182): A critical vulnerability in the React Flight protocol (used by the App Router)
that allows remote code execution (RCE) via crafted attacker-controlled requests.
```
