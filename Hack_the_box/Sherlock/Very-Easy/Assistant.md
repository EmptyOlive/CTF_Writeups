# Assistant

# Community Rating
**Easy**

# Scenario 
We have reason to believe that a QA Engineer had his credentials leaked, as his PC was accessed remotely without authorization. You are provided with PCA logs from the compromised Windows 11 system. As a junior analyst, your senior has asked you to establish a minimal timeline, which can then be filled in by detailed analysis afterwards.

# Hash Values
*c667dd0877f5176f55556c014e7f736992b0aa4f97c2a8299a70827c65f6669b* - Assistant.zip
*

# Questions
1. The attacker who gained unauthorized RDP access downloaded and installed an archive utility. What is the full path of the downloaded setup?
Answer - C:\Users\cyberjunkie\Downloads\7z2600-x64.exe

Solver - Found first looking at PcaGeneraldb and finding the malicious.
<img width="1894" height="553" alt="image" src="https://github.com/user-attachments/assets/c4afb417-9559-417b-8497-6ed8b911fc83" />

From there we pivot to PcaAppLaunch to find the corresponding log. 
<img width="1671" height="450" alt="image" src="https://github.com/user-attachments/assets/6b49dc6c-1636-4c65-b404-28165d0b4f47" />

2. The setup initially failed but succeeded in installing the utility the second time. When was it successfully installed?
Answer - 2026-04-08 11:30:39

Solver - Found in the pcaGeneraldb and seen directly after the failed installation. 
<img width="1894" height="553" alt="image" src="https://github.com/user-attachments/assets/72e03453-3014-4532-ad37-e437d068e1df" />

3. Following setting up collection ability, a Command Line tool for potential data exfiltration was also downloaded. What is the tool name and version?
Answer - rclone v1.73.3

Solver - Found in the pcaGeneraldb0, under the malicious program details.
<img width="1894" height="553" alt="image" src="https://github.com/user-attachments/assets/a4c01643-5572-4246-b83c-11cf74712cfb" />

4. The downloaded tool is CLI-based, but it was executed via GUI by the attacker, probably by mistake, which left evidence of execution in the PCA logs. Following this, the threat actor killed the process since it would not work via GUI interaction. When was the process killed?
Answer - 2026-04-08 11:32:08

Solver - Found in the pcaGeneraldb0 where the application was ran, cancelled then ran again.
<img width="1894" height="553" alt="image" src="https://github.com/user-attachments/assets/70df0198-a684-4e70-86ba-6af06b58f784" />

5. When did threat actor start the data collection process based on the limited evidence we have?
Answer - 2026-04-08 11:37:07

Solver - Found in the AppLaunch information. as shown from the photos this is the collection, exfiltration and cleaning
<img width="1671" height="450" alt="image" src="https://github.com/user-attachments/assets/a88f6a72-cd03-42b9-9f9c-5a0a20614a9c" />]

6. Performing temporal proximity analysis, how many seconds did the attacker take to conclude data collection before potentially exfiltrating it?
Answer - 
