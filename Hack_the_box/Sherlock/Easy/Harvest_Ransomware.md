# Harvest Ransomware

# Difficulty
**Easy**

# Scenario
Your incident-response team recovered a suspicious Linux executable named harvest from a compromised server. The sample appears to collect host information, upload data over FTP, and encrypt files before leaving a ransom note.

# File Hashes 
**aad658235f3984eca3f12675989997714f6e4c0c212d3e4effa7aa5044b5a9d7** - *Zip File*
**54f17a9549c48a2c27c061ef16d1c51d89cc19d32b3b71b772ab531f296a4376** - Harvest Malware File


# Tasks 

Task 1 - What is the hardcoded 4-digit victim ID embedded in the binary and used to name its on-disk artifacts?
Answer - 1606

Found through searching through the binary file till I found the UID input and where it is called on. 

<img width="303" height="337" alt="image" src="https://github.com/user-attachments/assets/84acb850-0520-4676-a4c0-65e08a6aa527" />

Task 2 - What is the full absolute path of the file where the malware writes collected system information before exfiltration?
Answer - 


