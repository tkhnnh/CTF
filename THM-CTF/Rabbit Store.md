# Overview
<img width="781" height="145" alt="Screenshot_2025-10-04_09-05-30" src="https://github.com/user-attachments/assets/3aba889b-e8f4-4431-bf60-1f47551ca8a0" />

# Reconnaisance
Repeating the same technique as usual, I kick start enumeration phase with TCP scan via `nmap`.
```
nmap -T4 -vvv -A -O -sV 10.201.119.75
...
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 61 OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3f:da:55:0b:b3:a9:3b:09:5f:b1:db:53:5e:0b:ef:e2 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBXuyWp8m+y9taS8DGHe95YNOsKZ1/LCOjNlkzNjrnqGS1sZuQV7XQT9WbK/yWAgxZNtBHdnUT6uSEZPbfEUjUw=
|   256 b7:d3:2e:a7:08:91:66:6b:30:d2:0c:f7:90:cf:9a:f4 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILcGp6ztslpYtKYBl8IrBPBbvf3doadnd5CBsO+HFg5M
80/tcp open  http    syn-ack ttl 61 Apache httpd 2.4.52
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://cloudsite.thm/
|_http-server-header: Apache/2.4.52 (Ubuntu)
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.15
OS details: Linux 4.15
...
```
Port 22 and 80 are open, but the service HTTP of port 80 does not allow me to directly access the service. Therefore, I have to add the ip and domain `cloudsite.thm` of target into my host in order to access the site.
```
sudo nano /etc/hosts
```
After adding target to my machine, I continue my scanning process by scanning vulnerability of this server with `nmap` as well
```
nmap --script=vuln -T5 10.201.119.75
...
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-fileupload-exploiter: 
|   
|     Couldn't find a file-type field.
|   
|_    Couldn't find a file-type field.
| http-sql-injection: 
|   Possible sqli for queries:
|     http://cloudsite.thm:80/assets/js/?C=D%3BO%3DA%27%20OR%20sqlspider
|     http://cloudsite.thm:80/assets/js/?C=S%3BO%3DA%27%20OR%20sqlspider
|     http://cloudsite.thm:80/assets/js/?C=M%3BO%3DA%27%20OR%20sqlspider
|     http://cloudsite.thm:80/assets/js/?C=N%3BO%3DD%27%20OR%20sqlspider
|     http://cloudsite.thm:80/assets/js/?C=S%3BO%3DA%27%20OR%20sqlspider
|     http://cloudsite.thm:80/assets/js/?C=D%3BO%3DD%27%20OR%20sqlspider
|     http://cloudsite.thm:80/assets/js/?C=M%3BO%3DA%27%20OR%20sqlspider
|     http://cloudsite.thm:80/assets/js/?C=N%3BO%3DA%27%20OR%20sqlspider
|     http://cloudsite.thm:80/assets/js/?C=D%3BO%3DA%27%20OR%20sqlspider
|     http://cloudsite.thm:80/assets/js/?C=S%3BO%3DD%27%20OR%20sqlspider
|     http://cloudsite.thm:80/assets/js/?C=M%3BO%3DA%27%20OR%20sqlspider
|_    http://cloudsite.thm:80/assets/js/?C=N%3BO%3DA%27%20OR%20sqlspider
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.


```

It might be vulnerable to SQLi, but I save it later, I need to find hidden endpoints of this target using `ffuf`
```
ffuf -c -r -u 'http://10.201.119.75/FUZZ' -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
...
assets                  [Status: 200, Size: 2487, Words: 162, Lines: 25, Duration: 333ms]
javascript              [Status: 403, Size: 278, Words: 20, Lines: 10, Duration: 332ms]

```

Digging further on `/assets`
```
ffuf -c -r -u 'http://cloudsite.thm/assets/FUZZ' -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
...
images                  [Status: 200, Size: 3171, Words: 173, Lines: 28, Duration: 338ms]
plugins                 [Status: 200, Size: 1175, Words: 76, Lines: 18, Duration: 337ms]
css                     [Status: 200, Size: 2611, Words: 140, Lines: 25, Duration: 335ms]
font                    [Status: 200, Size: 1166, Words: 73, Lines: 18, Duration: 337ms]
fonts                   [Status: 200, Size: 5175, Words: 289, Lines: 36, Duration: 341ms]
js                      [Status: 200, Size: 2403, Words: 131, Lines: 24, Duration: 335ms]
...
```

I am kinda stuck since there are not any useful information, even conducting a scan for session with `enum4linux` respond with nothing. There would be more ports which are uncommon, I got port 25672 while I was scanning and the nmap just cancelled due to time out
```
nmap -T5 -p25672 -A  -vv 10.201.15.144
...
PORT      STATE SERVICE REASON         VERSION
25672/tcp open  unknown syn-ack ttl 61
...
```

Carefully pay attention to the page source, I find another domain name of this server
<img width="1109" height="28" alt="Screenshot_2025-10-04_15-44-02" src="https://github.com/user-attachments/assets/7bf707ee-9544-4772-ba71-58796787ce76" />
<img width="620" height="746" alt="Screenshot_2025-10-04_15-50-04" src="https://github.com/user-attachments/assets/0bbff6ac-3cdc-4ed9-8aee-9a8646164d0d" />

I created a fake account to get in and came across this warning sign
<img width="1920" height="807" alt="Screenshot_2025-10-04_15-49-43" src="https://github.com/user-attachments/assets/dc6ab8d0-adf3-42c2-8bef-cc98f3b77fd5" />

That tells me that I need to tamper with the request to activate my subscription
<img width="1259" height="678" alt="Screenshot_2025-10-04_15-55-46" src="https://github.com/user-attachments/assets/09b69342-4f3e-44f5-85c2-d9c27e237460" />

Now, a new endpoint `api`, I need to find the rest endpoints
```
gobuster dir -u 'http://storage.cloudsite.thm/api/' -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 64
...
/register             (Status: 405) [Size: 36]
/login                (Status: 405) [Size: 36]
/docs                 (Status: 403) [Size: 27]
/uploads              (Status: 401) [Size: 32]
/Login                (Status: 405) [Size: 36]
/Docs                 (Status: 403) [Size: 27]
/Register             (Status: 405) [Size: 36]
/Uploads              (Status: 401) [Size: 32]
/DOCS                 (Status: 403) [Size: 27]
...
```
I cannot normally reach to the endpoints, so I decide to use curl to view them on the terminal
