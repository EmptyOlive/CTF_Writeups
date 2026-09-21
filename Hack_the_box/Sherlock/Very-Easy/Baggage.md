# Synopsis
This Sherlock provides players with an opportunity to analyze Shellbag artifacts. Shellbags can be used to find evidence of folder access by a specific user, access to network shares, and navigation of archive file contents. This information can be leveraged during investigations to identify potential data access, data staging, and data exfiltration attempts.


# Difficulty
**Very Easy**

# Tasks

Task 1 - What was the name of the archive file downloaded by the compromised account?
Answer - 1.zip

Task 2 - What was the name of the utility brought in by the attacher to search for sensitive data?
Answer - Everything 1.4.1.1028

Task 3 - The attacker navigated the filesystem and found sensitive files used by the victim in their day-to-day work. When was the VPN folder accessed by the attacker?
Answer - 2025-09-03 07:31:05
(Using Eric Zimmers ShellBags Explorer)[https://ericzimmerman.github.io/#forensic-tools]

Task 4 - What was the name of the directory containing the victim's passwords?
Answer - OnePassword MasterPass

Task 5 - The attacker also accessed a network share to pillage network data. What is the UNC path?
Answer - \\prod-ns-2\prodshare

Task 6 - When is the dam construction planned?
Answer - 2027

Task 7 - What was the name of the archive file present on the network share?
Answer - Dam Construction Engineer Plans.zip

Task 8 - When was the archive file from the network share accessed?
Answer - 2025-09-03 07:34:04

Task 9 - The attacker created a staging folder to prepare for collection and exfiltration. What is the full path of the staging folder?
Answer - C:\users\Steve\Pictures\a

Task 10 - The attacker compressed the staging folder to prepare the data for exfiltration. When was the exfiltration archive file accessed?
Answer - 2025-09-03 07:34:30
