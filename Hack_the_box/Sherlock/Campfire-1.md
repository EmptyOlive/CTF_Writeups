**CampFire1**

# Synopsis
Alonzo Spotted Weird files on his computer and informed the newly assembled SOC Team. Assessing the situation it is believed a Kerberoasting attack may have occurred in the network. It is your job to confirm the findings by analyzing the provided evidence.

You are provided with:

1- Security Logs from the Domain Controller

2- PowerShell-Operational Logs from the affected workstation

3- Prefetch Files from the affected workstation

# Difficulty
Very Easy 

# Tasks 

Task 1 - Analyzing Domain Controller Security Logs, can you confirm the UTC date & time when the kerberoasting activity occurred?
Answer - 2024-05-21 03:18:09

Task 2 - What is the Service Name that was targeted?
Answer - MSSQLService

Task 3 - It is really important to identify the Workstation from which this activity occurred. What is the IP Address of the workstation?
Answer - 172.17.79.129

Task 4 - Now that we have identified the workstation, a triage including PowerShell logs and Prefetch files are provided to you for some deeper insights so we can understand how this activity occurred on the endpoint. What is the name of the file used to Enumerate Active directory objects and possibly find Kerberoastable accounts in the network?
Answer - powerview.ps1

Task 5 - When was this script executed? (UTC)
Answer - 2024-05-21 03:16:32

Task 6 - What is the full path of the tool used to perform the actual kerberoasting attack?
Answer - C:\Users\Alonzo.spire\Downloads\Rubeus.exe

Task 7 - When was the tool executed to dump credentials? (UTC)
Answer - 2024-05-21 03:18:08
