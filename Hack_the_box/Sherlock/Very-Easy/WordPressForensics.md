# Word Press Forensics

# Scenerio
You are a DFIR (Digital Forensics and Incident Response) Analyst working for a corporation. A recent server breach has caused significant disruption in your digital environment. You are provided with a Splunk instance loaded with Apache2 access and error logs to investigate how the incident occurred.

Allow 1-2 minutes after spawning for the services to start and then connect to the SIEM using http://<ip>:8000

# Questions 

1. What is the attacker’s IP address from which the WPScan enumeration originated?
A. 203.101.190.116
Found through the clientip field, seeing that most of the requests are from this device it is safe to narrow down to look at this first

<img width="755" height="521" alt="image" src="https://github.com/user-attachments/assets/caf751f8-dc83-46ea-9413-42d319d9962b" />

clicking on this IP we see that there was a lot of WPS(Word Press Service) scans happening 
<img width="1449" height="166" alt="image" src="https://github.com/user-attachments/assets/01c6489a-5d7a-40cc-93da-6a4edc1c5457" />


2. When did the attacker begin reconnaissance activity?
A. 2024-08-28 03:15:23

Found through zooming into the timeline of first incident 
