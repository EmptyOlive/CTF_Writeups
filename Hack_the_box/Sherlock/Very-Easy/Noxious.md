**Noxious**

# Synposis
The IDS device alerted us to a possible rogue device in the internal Active Directory network. The Intrusion Detection System also indicated signs of LLMNR traffic, which is unusual. It is suspected that an LLMNR poisoning attack occurred. The LLMNR traffic was directed towards Forela-WKstn002, which has the IP address 172.17.79.136. A limited packet capture from the surrounding time is provided to you, our Network Forensics expert. Since this occurred in the Active Directory VLAN, it is suggested that we perform network threat hunting with the Active Directory attack vector in mind, specifically focusing on LLMNR poisoning.

# Difficulty
Very Easy

# Tasks

Task 1 - Its suspected by the security team that there was a rogue device in Forela's internal network running responder tool to perform an LLMNR Poisoning attack. Please find the malicious IP Address of the machine.
Answer - 172.17.79.135

Task 2 - What is the hostname of the rogue machine?
Answer - kali

Task 3 - Now we need to confirm whether the attacker captured the user's hash and it is crackable!! What is the username whose hash was captured?
Answer - john.deacon

Task 4 - In NTLM traffic we can see that the victim credentials were relayed multiple times to the attacker's machine. When were the hashes captured the First time?
Answer - 2024-06-24 11:18:30

Task 5 - What was the typo made by the victim when navigating to the file share that caused his credentials to be leaked?
Answer - DCC01

Task 6 - To get the actual credentials of the victim user we need to stitch together multiple values from the ntlm negotiation packets. What is the NTLM server challenge value?
Answer - 601019d191f054f1

Task 7 - Now doing something similar find the NTProofStr value.
Answer - c0cc803a6d9fb5a9082253a04dbd4cd4

Task 8 - To test the password complexity, try recovering the password from the information found from packet capture. This is a crucial step as this way we can find whether the attacker was able to crack this and how quickly.
Answer - NotMyPassword0K?
** NOTE**
```
Rockyou word list has updated since box release, need to check write up
```

Task 9 - Just to get more context surrounding the incident, what is the actual file share that the victim was trying to navigate to?
Answer - \\DC01\DC-Confidential
