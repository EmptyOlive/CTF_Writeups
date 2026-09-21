** Phishing_Email**

# Synopsis

Your email address has been leaked and you receive an email from Paypal in German. Try to analyze the suspicious email.

Connect to the VM with the credential provided using RDP. 
From pwnbox use the command 
xfreerdp /v:<ipaddress> /u:letsdefend /p:'' /cert:ignore /dynamic-resolution 
File location: C:\Users\LetsDefend\Desktop\Files\PhishingChallenge.zip Password: infected
This challenge prepared by @Fuuji


# Tasks

Task 0 - What is the return path of the email?
Answer - bounce@rjttznyzjjzydnillquh.designclub.uk.com

Task 1 - What is the domain name of the url in this mail?
Answer - storage.googleapis.com

Task 2 - Is the domain mentioned in the previous question suspicious?
Answer - Yes 

Task 3 - What is the sender IP address listed in the Received-SPF header?
Answer - 134.195.196.43 

Task 4 - Is this email a phishing email?
Answer - Yes
