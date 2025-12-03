# Recon 
## TCP scan
```
nmap -sV -T4 -vv -O -A 10.48.149.39
...
ORT     STATE SERVICE  REASON         VERSION
22/tcp   open  ssh      syn-ack ttl 62 OpenSSH 9.6p1 Ubuntu 3ubuntu13.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3a:8c:aa:d1:c2:7d:35:a2:4f:63:ee:18:9e:4c:db:49 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJZIUa7BZ427vgRujJuPhmrG8ZkObZBBL6in4nEKk13N2bWmsM07+H2QpOkEVBmLQA9Y1vCyZOHWpSc2dydomVE=
|   256 8e:d3:92:e6:74:dc:9d:cf:af:fa:55:23:35:32:3c:ee (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHnYSznfBD1ILm+utjl4DNm3DWbU8Z3sDIbql+7XUmgw
80/tcp   open  http     syn-ack ttl 62 nginx 1.24.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD
|_http-title: HopSec Asylum - Security Console
|_http-server-header: nginx/1.24.0 (Ubuntu)
8000/tcp open  http-alt syn-ack ttl 61
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
| http-title: Fakebook - Sign In
|_Requested resource was /accounts/login/?next=/posts/
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.0 404 Not Found
|     Content-Type: text/html
|     X-Frame-Options: DENY
|     Content-Length: 179
|     Vary: Accept-Language
|     Content-Language: en
|     X-Content-Type-Options: nosniff
|     <!doctype html>
|     <html lang="en">
|     <head>
|     <title>Not Found</title>
|     </head>
|     <body>
|     <h1>Not Found</h1><p>The requested resource was not found on this server.</p>
|     </body>
|     </html>
|   GenericLines, Help, RTSPRequest, SIPOptions, Socks5, TerminalServerCookie: 
|     HTTP/1.1 400 Bad Request
|   GetRequest, HTTPOptions: 
|     HTTP/1.0 302 Found
|     Content-Type: text/html; charset=utf-8
|     Location: /posts/
|     X-Frame-Options: DENY
|     Content-Length: 0
|     Vary: Accept-Language
|     Content-Language: en
|_    X-Content-Type-Options: nosniff
8080/tcp open  http     syn-ack ttl 62 SimpleHTTPServer 0.6 (Python 3.12.3)
|_http-title: HopSec Asylum - Security Console
| http-methods: 
|_  Supported Methods: GET HEAD
|_http-server-header: SimpleHTTP/0.6 Python/3.12.3
```
There are 4 open port
- 22 (ssh)
- 80 (HTTP)
- 8080 (HTTP)
- 8000 (HTTP)

## Port 80
<img width="1906" height="802" alt="Screenshot_2025-12-03_03-56-01" src="https://github.com/user-attachments/assets/23321db4-2faf-4f0a-8186-cada3c963d86" />

Except for all code of the login page, I found extra hidden code 
<img width="1903" height="759" alt="Screenshot_2025-12-03_03-57-32" src="https://github.com/user-attachments/assets/793ee98e-fe28-4b4d-8f27-6e1d169d7b59" />

Next, I am going to find hidden paths of this service
```
ffuf -u http://10.48.149.39/FUZZ -w /usr/share/wordlists/dirb/big.txt 2>/dev/null   
cgi-bin                 [Status: 301, Size: 178, Words: 6, Lines: 8, Duration: 177ms]
```

## Port 8000
<img width="1898" height="842" alt="Screenshot_2025-12-03_04-03-09" src="https://github.com/user-attachments/assets/8af090fc-30ea-46d4-b3a7-3cd5aed6c5c0" />

This turns out to be a replicate facebook sign-up page, here are its hidden paths
```
ffuf -u http://10.48.149.39:8000/FUZZ -w /usr/share/wordlists/dirb/big.txt 2>/dev/null
admin                   [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 217ms]
chat                    [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 265ms]
media                   [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 197ms]
posts                   [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 224ms]
profiles                [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 224ms]
```
So I creating a fake account to get in, but first I check whether username  `admin` exist or not
<img width="1906" height="777" alt="Screenshot_2025-12-03_04-06-04" src="https://github.com/user-attachments/assets/a01dd952-e50d-43b5-b2d0-f3ac2bf7caef" />

And after logging, I found some users which strengthen my earlier claim
<img width="1910" height="737" alt="Screenshot_2025-12-03_04-20-07" src="https://github.com/user-attachments/assets/0a21f433-8e60-4eb0-9087-da87f2739df9" />

User `Sir Carrotbane` mention brute-force which sheds light on brute-forcing for finding password for below credentials
Credentials:
 `guard.hopkins@hopsecasylum.com` 
 `admin`
 <img width="1790" height="619" alt="Screenshot_2025-12-03_04-22-04" src="https://github.com/user-attachments/assets/17d2e3c6-8d68-43d7-bbb1-3682e2663292" />
so if I comment my password on someone post will it decrypt my password -> no



