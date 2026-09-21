Operation Blackout 2025: Phantom Check


# Synopsis
Talion suspects that the threat actor carried out anti-virtualization checks to avoid detection in sandboxed environments. Your task is to analyze the event logs and identify the specific techniques used for virtualization detection. Byte Doctor requires evidence of the registry checks or processes the attacker executed to perform these checks.

# Difficulty
Very Easy

# Tasks 

Task 1 - Which WMI class did the attacker use to retrieve model and manufacturer information for virtualization detection?
Answer - Win32_ComputerSystem

Task 2 - Which WMI query did the attacker execute to retrieve the current temperature value of the machine?
Answer - SELECT * FROM MSAcpi_ThermalZoneTemperature

Task 3 - The attacker loaded a PowerShell script to detect virtualization. What is the function name of the script?
Answer - Check-VM

Task 4 - Which registry key did the above script query to retrieve service details for virtualization detection?
Answer - HKLM:\SYSTEM\ControlSet001\Services

Task 5 - The VM detection script can also identify VirtualBox. Which processes is it comparing to determine if the system is running VirtualBox?
Answer - vboxservice.exe, vboxtray.exe

Task 6 - The VM detection script prints any detection with the prefix 'This is a'. Which two virtualization platforms did the script detect?
Answer - Hyper-V, Vmware
