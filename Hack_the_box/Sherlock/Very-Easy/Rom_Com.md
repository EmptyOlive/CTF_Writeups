**RomCom**

# Synopsis

Sherlock Scenario
Susan works at the Research Lab in Forela International Hospital. A Microsoft Defender alert was received from her computer, and she also mentioned that while extracting a document from the received file, she received tons of errors, but the document opened just fine. According to the latest threat intel feeds, WinRAR is being exploited in the wild to gain initial access into networks, and WinRAR is one of the Software programs the staff uses. You are a threat intelligence analyst with some background in DFIR. You have been provided a lightweight triage image to kick off the investigation while the SOC team sweeps the environment to find other attack indicators.


# Tasks 

Task 1 - What is the CVE assigned to the WinRAR vulnerability exploited by the RomCom threat group in 2025?
Answer - CVE-2025-8088

Task 2 - What is the nature of this vulnerability?
Answer - path traversal

Task 3 - What is the name of the archive file under Susan's documents folder that exploits the vulnerability upon opening the archive file?
Answer - Pathology-Department-Research-Records.rar

Task 4 - When was the archive file created on the disk?
Answer - 2025-09-02 08:13:50

Task 5 - When was the archive file opened?
Answer - 2025-09-02 08:14:04

** When investigating this type of incident, the timestamp differences come down to a common artifact nuance during a forensic analysis.
1. Ingest Latency & Rounding RulesThe exact execution and file metadata modifications captured in the $MFT record for the Last Record Change (0x10) attribute register the exact closing of the initial handle at 08:14:04. If a tool or an unzipping process triggers downstream file births (like the malicious dropping of the backdoor payload or extracted decoy files), those processes register on the next clock tick at 08:14:05.2. The USN Journal vs. MFT DelaysIf you are looking at the Update Sequence Number (USN) Journal ($J), it records changes in absolute real-time stream sequences. The $MFT master table attributes update when the transaction buffer flushes. Because of this, it is highly common for a file creation or extraction modification to look slightly offset by exactly 1 second between different tracking tables.**

Task 6 - What is the name of the decoy document extracted from the archive file, meant to appear legitimate and distract the user?
Answer - Genotyping_Results_B57_Positive.pdf

Task 7 - What is the name and path of the actual backdoor executable dropped by the archive file?
Answer - C:\Users\Susan\Appdata\Local\ApbxHelper.exe

Task 8 - The exploit also drops a file to facilitate the persistence and execution of the backdoor. What is the path and name of this file?
Answer - C:\Users\Susan\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\Display Settings.lnk

Task 9 - What is the associated MITRE Technique ID discussed in the previous question?
Answer - T1547.009

Task 10 - When was the decoy document opened by the end user, thinking it to be a legitimate document?
Answer - 2025-09-02 08:15:05 

