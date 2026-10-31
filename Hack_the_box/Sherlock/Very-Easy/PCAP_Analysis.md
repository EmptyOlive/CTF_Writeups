# PCAP_Analysis

# Difficulty 
**Easy**

# Synopsis
We have captured this traffic from P13's computer. Can you help him?

# Document hashes
*1c30999cc3473342db6f6a0c655a8f90cf499980111a4c545104b02e963bb500* - **PCAP_Analysis.zip**
*97e9b5e4a2c0e5bbc924f5d4edf5c5f67951f770059d69f5c6f820ecc932865b* - **Pcap_analysis.pcapng**

# Tasks
Task 1 - In network communication, what are the IP addresses of the sender and receiver?
Answer - 192.168.235.137,192.168.235.131

Solver - 
using the filter 
```
frame contains "P13"
```
we see the following 
<img width="1234" height="211" alt="image" src="https://github.com/user-attachments/assets/f1b89de6-f1a1-45b4-a46f-fe2d0cafdade" />
then clicking follow TCP stream we see 
<img width="977" height="548" alt="image" src="https://github.com/user-attachments/assets/228d25e9-c1a2-4943-95dd-df6d7fc303ba" />

Task 2 - P13 uploaded a file to the web server. What is the IP address of the server?
Answer - 192.168.1.7

Solver -
Found using the http filter then seeing the POST request as we know P13's IP
<img width="891" height="108" alt="image" src="https://github.com/user-attachments/assets/74ff3a1a-8889-4aab-852e-ddb8d00c4851" />

Task 3 - What is the name of the file that was sent through the network?
Answer - file 

Solver -
Found looking at the response frame from the POST request from P13 in the data sub section
<img width="934" height="186" alt="image" src="https://github.com/user-attachments/assets/8e775a3c-d4a5-4b59-b75a-5f2f6fd210f3" />
<img width="943" height="374" alt="image" src="https://github.com/user-attachments/assets/4fba8035-d5dd-428f-b0eb-d0303b1e34cb" />

Task 4 - What web server software is running on the target machine?
Answer - Apache

Solver - Found in the same response frame as Task 3, dropping down the HTTP header we see the key details 
<img width="572" height="278" alt="image" src="https://github.com/user-attachments/assets/dc623f50-f1e1-46bc-8ac2-349c0541480a" />

Task 5 - What directory was the file uploaded to?
Answer - Uploads

Solver - 
Found again in the HTTP 200 response 
<img width="624" height="262" alt="image" src="https://github.com/user-attachments/assets/d8af18b2-1bd0-44ce-a79e-b1fca83199ca" />

Task 6 - How long did it take the sender to send the encrypted file?
Answer - 0.0073 

Solver - Found going to statistics then conversations and looking for the connection between P13 and the server
<img width="1639" height="287" alt="image" src="https://github.com/user-attachments/assets/1fda2ad1-6610-4f1b-8d94-e56e340c9369" />







