**CampFire2**

# Synopsis

Forela's Network is constantly under attack. The security system raised an alert about an old admin account requesting a ticket from KDC on a domain controller. Inventory shows that this user account is not used as of now so you are tasked to take a look at this. This may be an AsREP roasting attack as anyone can request any user's ticket which has preauthentication disabled.

# Difficulty

**Very Easy**

# Tasks

Task 1 - When did the ASREP Roasting attack occur, and when did the attacker request the Kerberos ticket for the vulnerable user?
Answer - 2024-05-29 06:36:40

Task 2 - Please confirm the User Account that was targeted by the attacker.
Answer - arthur.kyle

Task 3 - What was the SID of the account?
Answer - S-1-5-21-3239415629-1862073780-2394361899-1601

Task 4 - It is crucial to identify the compromised user account and the workstation responsible for this attack. Please list the internal IP address of the compromised asset to assist our threat-hunting team.
Answer - 172.17.79.129


Task 5 - We do not have any artifacts from the source machine yet. Using the same DC Security logs, can you confirm the user account used to perform the ASREP Roasting attack so we can contain the compromised account/s?
Answer - happy.grunwald
