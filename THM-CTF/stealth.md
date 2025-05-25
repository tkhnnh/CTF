# Stealth Write-Up
![Image](https://github.com/user-attachments/assets/a026b3ff-0a6c-4392-adc4-d2a233f24513)
## Enumeration

```
b0xb0x@Bmo  ~/stealth :  nmap -T4 -sV  -v -O -A -p-  10.10.81.212  -oN nmap.txt
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-25 21:37 AEST
...
PORT      STATE SERVICE       VERSION
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2025-05-25T11:45:43+00:00; -1s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: HOSTEVASION
|   NetBIOS_Domain_Name: HOSTEVASION
|   NetBIOS_Computer_Name: HOSTEVASION
|   DNS_Domain_Name: HostEvasion
|   DNS_Computer_Name: HostEvasion
|   Product_Version: 10.0.17763
|_  System_Time: 2025-05-25T11:45:06+00:00
| ssl-cert: Subject: commonName=HostEvasion
| Issuer: commonName=HostEvasion
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-05-24T11:34:23
| Not valid after:  2025-11-23T11:34:23
| MD5:   eed7:4955:e4db:0f63:302f:e7b2:d1f1:6c49
|_SHA-1: eef6:e513:c510:15a4:0146:b3d4:0316:2e2f:fe24:1a68
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
8000/tcp  open  http          PHP cli server 5.5 or later
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: 404 Not Found
8080/tcp  open  http          Apache httpd 2.4.56 ((Win64) OpenSSL/1.1.1t PHP/8.0.28)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: PowerShell Script Analyser
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: Apache/2.4.56 (Win64) OpenSSL/1.1.1t PHP/8.0.28
8443/tcp  open  ssl/http      Apache httpd 2.4.56 ((Win64) OpenSSL/1.1.1t PHP/8.0.28)
| ssl-cert: Subject: commonName=localhost
| Issuer: commonName=localhost
| Public Key type: rsa
| Public Key bits: 1024
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2009-11-10T23:48:47
| Not valid after:  2019-11-08T23:48:47
| MD5:   a0a4:4cc9:9e84:b26f:9e63:9f9e:d229:dee0
|_SHA-1: b023:8c54:7a90:5bfa:119c:4e8b:acca:eacf:3649:1ff6
|_http-title: PowerShell Script Analyser
|_http-server-header: Apache/2.4.56 (Win64) OpenSSL/1.1.1t PHP/8.0.28
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49674/tcp open  msrpc         Microsoft Windows RPC
...
```
I had 3 noticable ports `8080`, `8443`,`3389`

Access the port `8080` first
![Image](https://github.com/user-attachments/assets/7333cac3-f8d3-49f6-82ee-da9e4f875571)
Look like this site allow upload file but with `.ps1` extentsion

Another port `8443`
![Image](https://github.com/user-attachments/assets/a2659a07-4bfa-4fbd-8399-1efc34196b7a)
I had bad request for this port

I tried another one port `8000`
![Image](https://github.com/user-attachments/assets/e9c4dd3d-faab-4308-b802-cea52d97b793)
It was not found therefore this port might still have something
