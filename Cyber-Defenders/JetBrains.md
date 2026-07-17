# Scenario
During a recent security incident, an attacker successfully exploited a vulnerability in our web server, allowing them to upload webshells and gain full control over the system.
The attacker utilized the compromised web server as a launch point for further malicious activities, including data manipulation. 

As part of the investigation, You are provided with a packet capture (PCAP) of the network traffic during the attack to piece together the attack timeline and identify the methods used by the attacker.
The goal is to determine the initial entry point, the attacker's tools and techniques, and the compromise's extent.

# Community Rating
Easy 

# Questions
1. Identifying the attacker's IP address helps trace the source and stop further attacks. What is the attacker's IP address?
Answer - 23.158.56.196

2.To identify potential vulnerability exploitation, what version of our web server service is running?
Answer - 203.11.3

3. After identifying the version of our web server service, what CVE number corresponds to the vulnerability the attacker exploited?
Answer - CVE-2024-27198

4.  The attacker exploited the vulnerability to create a user account. What credentials did he set up?
Answer - c91oyemw:CL5vzdwLuK

5. The attacker uploaded a webshell to ensure his access to the system. What is the name of the file that the attacker uploaded?
Answer - NSt8bHTg.zip

6. When did the attacker execute their first command via the web shell?
Answer - 2024-06-30 08:03

7. The attacker tampered with a text file that contained the credentials of the admin user of the webserver. What new username and password did the attacker write in the file?

