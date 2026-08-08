# Synopsis
🛎️ Concierge Briefing
He booked the quiet room. It's not on the floor plan, not in the brochure, not on any door. But port 8080 is wide open, and the rooms it never lists are the ones worth finding.

Welcome to the Byte Lotus, where the WiFi is open, the app is free, and the concierge already knows your coffee order. You spend these first days as a guest who simply notices things
— a room that isn't on the floor plan, packets that leave every night at the same hour, a profile assembled from two breakfasts and a livestream.

The Byte Lotus guest-experience platform went live in a hurry, and the night-shift developer shipped more than the website.

# Rating
Very Easy

# Method of Solve

 Going to the website we see there isn't much.

 Using buster scan we see 

 feroxbuster -u http://10.49.154.101:8080 -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
                                                                                                                                                                                                                                            
 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.13.1
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.49.154.101:8080/
 🚩  In-Scope Url          │ 10.49.154.101
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.13.1
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET        1l        3w       13c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET        5l       13w       92c http://10.49.154.101:8080/.git/config
200      GET        4l       11w      409c http://10.49.154.101:8080/.git/index
200      GET        1l        2w       21c http://10.49.154.101:8080/.git/HEAD
200      GET       13l       28w      437c http://10.49.154.101:8080/.git
200      GET        1l       13w      189c http://10.49.154.101:8080/.git/logs/HEAD
200      GET       52l      218w     2554c http://10.49.154.101:8080/
200      GET        3l        8w      153c http://10.49.154.101:8080/.git/logs/refs
200      GET        4l       10w      165c http://10.49.154.101:8080/.git/logs/
seeing there is a git folder. We then use gitdumper and from there we see a README.txt
Byte Lotus — Guest Experience Platform
 reading this we see
 ```
Internal staging repository for the guest app and concierge personalization
service. Do not deploy this folder to production.
    
Staging flag (remove before launch): THM{byt3_l0tus_n3v3r_f0rg3ts}
```
 

 **THM{byt3_l0tus_n3v3r_f0rg3ts}**
