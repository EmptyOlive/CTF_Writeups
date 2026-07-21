# Scenario 
An automated alert has detected unusual XML data being processed by the server, which suggests a potential XXE (XML External Entity) Injection attack.
This raises concerns about the integrity of the company's customer data and internal systems, prompting an immediate investigation.
Analyze the provided PCAP file using the network analysis tools available to you. Your goal is to identify how the attacker gained access and what actions they took.

# Community Difficulty
Easy

# Questions
1. Identifying the open ports discovered by an attacker helps us understand which services are exposed and potentially vulnerable.
 Can you identify the highest-numbered port that is open on the victim's web server?
Answer - 3306
<img width="1410" height="384" alt="image" src="https://github.com/user-attachments/assets/08bc55d2-06a9-49b7-a92d-a4111617cfe6" />

2. By identifying the vulnerable PHP script, security teams can directly address and mitigate the vulnerability. What's the complete URI of the PHP script vulnerable to XXE Injection?
Answer - /review/upload.php
   <img width="1029" height="192" alt="image" src="https://github.com/user-attachments/assets/e06242ef-ca2d-4830-b70f-a16a84eb2a80" />
   
3. To construct the attack timeline and determine the initial point of compromise. What's the name of the first malicious XML file uploaded by the attacker?
Answer - TheGreatGatsby.xml
<img width="941" height="648" alt="image" src="https://github.com/user-attachments/assets/56040be2-ee43-42ba-a4ef-04400d003f1c" />

4. Understanding which sensitive files were accessed helps evaluate the breach's potential impact. What's the name of the web app configuration file the attacker read?
Answer - config.php
<img width="940" height="719" alt="image" src="https://github.com/user-attachments/assets/4dd4fbfb-624f-4890-95b7-d0f192f1ace2" />

5. To assess the scope of the breach, what is the password for the compromised database user?
Answer - Winter2024
<img width="948" height="722" alt="image" src="https://github.com/user-attachments/assets/ea0b9c6e-ea7b-49f7-a57d-25d69899e8b1" />

6. Following the database user compromise. What is the timestamp of the attacker's initial connection to the MySQL server using the compromised credentials after the exposure?
Answer - 2024-05-31 12:08 (Important to look at timestamps)
<img width="954" height="431" alt="image" src="https://github.com/user-attachments/assets/52b031f9-72c3-4272-99a7-95679423a71f" />

7. To eliminate the threat and prevent further unauthorized access, can you identify the name of the web shell that the attacker uploaded for remote code execution and persistence?
Answer - booking.php
<img width="933" height="680" alt="image" src="https://github.com/user-attachments/assets/3bf586d1-f7de-4b40-93d5-14d8f718347f" />

