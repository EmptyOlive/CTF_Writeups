# Synopsis 
Your organization's SOC team intercepted a suspicious binary during a routine threat hunting operation on a Linux server. The file was found in /var/tmp with an unusual name and was attempting to establish outbound connections. Initial analysis suggests this could be a post-exploitation agent. Your task is to perform static analysis on the binary to identify its capabilities, extract indicators of compromise, and understand the threat actor's infrastructure.

# Difficulty 
Very Easy 


# Tasks

Task 1 - What is the SHA256 hash of the malicious binary?
Answer - 2d7b1b2178f76c26893b2a56cbf9b36700235259e76b893d53817d5b66b634a5

Task 2 - What is the IP address hardcoded in the binary for C2 communication?
Answer - 192.168.56.1

Task 3 - What port does the agent connect to on the C2 server?
Answer - 4445
