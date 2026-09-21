**Operation Blackout 2025 - Smoke & Mirrors**

# Synopsis
Byte Doctor Reyes is investigating a stealthy post-breach attack where several expected security logs and Windows Defender alerts appear to be missing. He suspects the attacker employed defense evasion techniques to disable or manipulate security controls, significantly complicating detection efforts.

Using the exported event logs, your objective is to uncover how the attacker compromised the system's defenses to remain undetected.

# Difficulty
**Very Easy**

# Tasks

Task 1 - The attacker disabled LSA protection on the compromised host by modifying a registry key. What is the full path of that registry key?
Answer - HKLM\SYSTEM\CurrentControlSet\Control\LSA

Task 2 - Which PowerShell command did the attacker first execute to disable Windows Defender?
Answer - Set-MpPreference -DisableIOAVProtection $true -DisableEmailScanning $true -DisableBlockAtFirstSeen $true

Task 3 - The attacker loaded an AMSI patch written in PowerShell. Which function in the DLL is being patched by the script to effectively disable AMSI?
Answer - AmsiScanBuffer

Task 4 - Which command did the attacker use to restart the machine in Safe Mode?
Answer - bcdedit.exe /set safeboot network

Task 5 - Which PowerShell command did the attacker use to disable PowerShell command history logging?
Answer - Set-PSReadlineOption -HistorySaveStyle SaveNothing


