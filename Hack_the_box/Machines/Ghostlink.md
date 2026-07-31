<img width="1612" height="698" alt="image" src="https://github.com/user-attachments/assets/cc1c8b29-4d89-4cf6-88d7-ab22bc114b82" />
<img width="800" height="800" alt="image" src="https://github.com/user-attachments/assets/f91b4ce1-c35f-495e-bd3b-3fbc2102e3d3" />


# Synopsis 
Ghostlink is a Hard difficulty Windows machine featuring an Active Directory domain controller and a web server. 
Enumeration reveals a critical MQTT service used for node tracking, which exposes two internal hosts: a secure file sharing app and a Gogs code host.
The attacker modifies the MQTT health check to trigger NTLM authentication, relaying credentials to authenticate as the svc_canary service account.
Using this authentication, the attacker exploits a double URL-encoded path traversal vulnerability to exfiltrate the service account's ntuser.dat file.
Analysis of the registry hive reveals a recent document for db.zip, containing KeePass credentials for the Gogs application. 
These credentials are then leveraged to exploit an RCE vulnerability CVE-2025-8110 in Gogs to obtain a foothold. Once on the system, the attacker cracks a Gogs hash to log in as the local user nvirelli.
Finally, the ESC11 vulnerability in ADCS allows the attacker to request a Domain Controller certificate and compromise the domain.

# Community Rating
Hard


# Method of Solve

***Enumeration***
```
PORT      STATE SERVICE       VERSION                                                                                                                                                                                                      
53/tcp    open  domain        Simple DNS Plus                                                                                                                                                                                              
80/tcp    open  http          Microsoft IIS httpd 10.0                                                                                                                                                                                     
| http-methods:                                                                                                                                                                                                                            
|_  Potentially risky methods: TRACE                                                                                                                                                                                                       
|_http-server-header: Microsoft-IIS/10.0                                                                                                                                                                                                   
|_http-title: Ghost Protocol Zero                                                                                                                                                                                                          
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-07-31 20:07:04Z)                                                                                                                                               
135/tcp   open  msrpc         Microsoft Windows RPC                                                                                                                                                                                        
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn                                                                                                                                                                                
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: ghostlink.htb, Site: Default-First-Site-Name)                                                                                                               
|_ssl-date: TLS randomness does not represent time                                                                                                                                                                                         
| ssl-cert: Subject: commonName=dc01.ghostlink.htb                                                                                                                                                                                         
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:dc01.ghostlink.htb                                                                                                                                          
| Not valid before: 2026-03-03T16:53:53                                                                                                                                                                                                    
|_Not valid after:  2027-03-03T16:53:53                                                                                                                                                                                                    
445/tcp   open  microsoft-ds?                                                                                                                                                                                                              
464/tcp   open  kpasswd5?                                                                                                                                                                                                                  
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0                                                                                                                                                                          
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: ghostlink.htb, Site: Default-First-Site-Name)                                                                                                               
|_ssl-date: TLS randomness does not represent time                                                                                                                                                                                         
| ssl-cert: Subject: commonName=dc01.ghostlink.htb                                                                                                                                                                                         
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:dc01.ghostlink.htb                                                                                                                                          
| Not valid before: 2026-03-03T16:53:53                                                                                                                                                                                                    
|_Not valid after:  2027-03-03T16:53:53                                                                                                                                                                                                    
1883/tcp  open  mqtt                                                                                                                                                                                                                       
| mqtt-subscribe:                                                                                                                                                                                                                          
|   Topics and their most recent payloads:                                                                                                                                                                                                 
|     $SYS/brokers/client_status/mqttui-b4228239: {"status":"online", "username":"(null)", "ts":1785528483112,"proto_name":"MQTT","keepalive":60,"return_code":"0","proto_ver":4,"client_id":"mqttui-b4228239","clean_start":1, "IPv4":"127
.0.0.1"}                                                                                                                                                                                                                                   
|     $SYS/brokers/client_status/mqttui-ebd828dd: {"status":"offline", "username":"(null)","ts":1785528480906,"reason_code":"0","client_id":"mqttui-ebd828dd","IPv4":"127.0.0.1"}                                                          
|_    $SYS/brokers/client_status/mqttui-e04f6faa: {"status":"offline", "username":"(null)","ts":1785528478473,"reason_code":"0","client_id":"mqttui-e04f6faa","IPv4":"127.0.0.1"}                                                          
2179/tcp  open  vmrdp?
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: ghostlink.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc01.ghostlink.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:dc01.ghostlink.htb
| Not valid before: 2026-03-03T16:53:53
|_Not valid after:  2027-03-03T16:53:53
|_ssl-date: TLS randomness does not represent time
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: ghostlink.htb, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc01.ghostlink.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:dc01.ghostlink.htb
| Not valid before: 2026-03-03T16:53:53
|_Not valid after:  2027-03-03T16:53:53
|_ssl-date: TLS randomness does not represent time
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49676/tcp open  msrpc         Microsoft Windows RPC
49677/tcp open  msrpc         Microsoft Windows RPC
49679/tcp open  msrpc         Microsoft Windows RPC
49680/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49902/tcp open  msrpc         Microsoft Windows RPC
49913/tcp open  msrpc         Microsoft Windows RPC
60933/tcp open  msrpc         Microsoft Windows RPC
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2022|11|2012 (87%)
OS CPE: cpe:/o:microsoft:windows_server_2022 cpe:/o:microsoft:windows_11 cpe:/o:microsoft:windows_server_2012:r2
Aggressive OS guesses: Microsoft Windows Server 2022 (87%), Microsoft Windows 11 24H2 (85%), Microsoft Windows Server 2012 R2 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time:
|   date: 2026-07-31T20:08:02
|_  start_date: N/A
|_clock-skew: 8h05m48s
| smb2-security-mode:
|   3.1.1:
|_    Message signing enabled and required

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   65.29 ms 10.10.14.1
2   65.69 ms ghostlink.academy.htb (10.129.2.37)
```
Looking at the website we get welcomed by 
<img width="1612" height="698" alt="image" src="https://github.com/user-attachments/assets/31ee76f9-4d09-4b96-85a0-fad5aa06feb5" />


After a web search there seems to be no foothold vulnerabilities for the IIS. So now we will move onto the mqtt service 
```
1883/tcp  open  mqtt                                                                                                                                                                                                                       
| mqtt-subscribe:                                                                                                                                                                                                                          
|   Topics and their most recent payloads:                                                                                                                                                                                                 
|     $SYS/brokers/client_status/mqttui-b4228239: {"status":"online", "username":"(null)", "ts":1785528483112,"proto_name":"MQTT","keepalive":60,"return_code":"0","proto_ver":4,"client_id":"mqttui-b4228239","clean_start":1, "IPv4":"127
.0.0.1"}                                                                                                                                                                                                                                   
|     $SYS/brokers/client_status/mqttui-ebd828dd: {"status":"offline", "username":"(null)","ts":1785528480906,"reason_code":"0","client_id":"mqttui-ebd828dd","IPv4":"127.0.0.1"}                                                          
|_    $SYS/brokers/client_status/mqttui-e04f6faa: {"status":"offline", "username":"(null)","ts":1785528478473,"reason_code":"0","client_id":"mqttui-e04f6faa","IPv4":"127.0.0.1"}
```




