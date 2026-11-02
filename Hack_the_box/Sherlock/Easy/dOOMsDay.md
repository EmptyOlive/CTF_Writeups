# dOOMs Day

# Difficulty
*Easy*

# Scenario
Suspicious traffic was observed on one of the internal Dev servers at DevNotes.org. Logs from the server and a packet capture from the subnet have been provided to you for analysis.

In this Sherlock, players will analyze a compromised server that was targeted using the PHP2Shell exploit. The Sherlock focuses on understanding how the exploit works and exploring all the patterns observed in the web logs.

# Hash Values
*c545003fddee363c678e7c4adcf3de43ec778afcfd53ccf27be696075c83e6ca* - dOOMsDay.zip
*fde7842ee645d975a7828df857b56e78bacb43ef2192d1717babfde7a54b8725* - Capture
*50a795dde1efd0d937b178e8af74d137a15f9b5f6885bb3e8790640ab1e00f2c* - Access.log
*82d323c726895e7cd8a98de7e97168a9e65940ff0e46c564e368b1366a7d0264* - error.log


# Tasks

Task 1 - What version of PHP was running on the server?
Answer - 8.4.26

Solver - 
Found using filter http.response then examining packets 
<img width="720" height="287" alt="image" src="https://github.com/user-attachments/assets/54b89367-9576-4954-9a9a-1adec63aa3d5" />

Task 2 - At what exact date and time did the attacker send the first request belonging to the exploitation attempt? Provide the timestamp of the earliest anomalous request from the attacker's IP address in the web server logs.
Answer - 2026-10-28 08:42:52

Solver - 

Found grepping the access log
```
grep '^192\.168\.72\.1 ' access.log | sort -t'[' -k2,2 | head
```
<img width="703" height="159" alt="image" src="https://github.com/user-attachments/assets/2b9cbfc1-ba17-4ea0-96a1-bc9bc4734792" />

Task 3 - To occupy the worker pool, the exploit sent stalled multipart uploads. What is the value of the boundary separator used in these requests?
Answer - ZZZZZZZZ

Solver - Found in the error log for the boundary.
<img width="1703" height="92" alt="image" src="https://github.com/user-attachments/assets/10fadb70-f047-4fa4-a683-2b286cc3ff13" />

Task 4 - The exploit deliberately occupied the Apache worker pool with stalled multipart uploads, forcing the exploit's requests to land on the same single worker so that memory is leaked from the same process heap. How many workers did the exploit deliberately occupy?
Answer - 149

Solver - Found grepping the error log using the following command 
```
grep -ic 'boundary=ZZZZZZZZZZ' error.log
```

Task 5 - Which two CVEs are chained together to achieve remote code execution?
Answer - 


