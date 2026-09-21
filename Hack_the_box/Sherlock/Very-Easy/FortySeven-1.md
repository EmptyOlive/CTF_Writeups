# Sherlock Scenario 
An APT group is using Hajj-themed phishing lures to target and steal WhatsApp data from government and diplomatic officials. Our team has gathered fragmented intelligence from public cybersecurity vendor reports, blog posts, and internal security alerts. Your task is to build a comprehensive profile of the threat actor responsible. You must connect the dots between different reports to answer questions about their identity, tools, and motives.

Below are the sites we have compiled, use these to answer the following questions. (If any of the sites become unavailable please check on wayback machine as they all have been saved).

Evidence - 1 - https://securelist.com/mysterious-elephant-apt-ttps-and-tools/117596/ Evidence - 2 - https://medium.com/@knownsec404team/apt-k-47-mysterious-elephant-a-new-apt-organization-in-south-asia-5c66f954477 Evidence - 3 - https://medium.com/@knownsec404team/unveiling-the-past-and-present-of-apt-k-47-weapon-asyncshell-5a98f75c2d68


# Community Rating 
Very Easy 


# Task 1 
What is the primary name of the APT group described in the SecureList report?
Answer - Mysterious Elephant

# Task 2 
According to the Knownsec 404 team's analysis(Evidence -3), since which year has this group's attack activity been dated back to?
Answer - 2022

# Task 3
The group uses a custom backdoor that communicates via Office Remote Procedure Call (ORPCBackdoor). According to the Knownsec 404 team's analysis(Evidence -2), what is the name of the first malicious exported entry function?
Answer - GetFileVersionInfoByHandleEx(void)

# Task 4
The previously mentioned backdoor checks for a file before creating persistence. What is the name of the file?
Answer - ts.dat

# Task 5
The use of the backdoor links the APT to another well-known South Asian APT group. What is the name of this other group?
Answer - Bitter

# Task 6
The APT group we are currently investigating has consistently used and updated another backdoor since 2023, with its C2 communication evolving from TCP to HTTPS. What is the name of this tool?
Answer - Asyncshell-v2 

# Task 7
To evade sandbox analysis, the MemLoader HidenDesk tool checks the number of active processes before running. What is the minimum number of processes required for it to proceed?
Answer - 40

# Task 8
The MemLoader HidenDesk tool creates a covert environment for its activities by creating and switching to a specific environment. What is the name of this hidden desktop?
Answer - MalwareTech_Hidden

# Task 9 
The MemLoader HidenDesk tool achieves persistence by placing a shortcut in the autostart folder to ensure it runs after a system reboot. What is the MITRE ATT&CK ID for the 'Registry Run Keys / Startup Folder' technique?

# Task 10
The actor uses several custom exfiltration tools targeting WhatsApp. What is the name of the tool that recursively searches specific directories, including the “Desktop” and “Downloads” folders?
Answer - Stom Exfiltrator

# Task 11
Kaspersky's analysis highlights the actor's heavy use of scripts for execution and deploying payloads. What is the MITRE ATT&CK ID for the 'PowerShell' technique?
Answer - T1059.001

# Task 12
In their early attack chains, Mysterious Elephant used a downloader that was previously associated with the Origami Elephant group. What was the name of this downloader?
Answer - Vtyrei

# Task 13 
In a January 2024 campaign delivering an Asyncshell payload, which CVE was exploited in the malicious archive file? Be careful to use a hyphen - and not an emdash — in the flag.
Answer - CVE-2023-38831

# Task 14
What is the MD5 hash of the ChromeStealer Exfiltrator sample named WhatsAppOB.exe?
Answer - 9e50adb6107067ff0bab73307f5499b6 

# Task 15 
The intelligence describes multiple custom tools designed to upload stolen data to the actor's servers. According to the MITRE ATT&CK framework, what is the ID for the 'Exfiltration Over C2 Channel' technique?
Answer - T1041

