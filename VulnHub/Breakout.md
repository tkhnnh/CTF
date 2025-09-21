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
