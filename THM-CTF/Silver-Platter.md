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
nmap -T4 -sV -O -A -v 10.10.9.219 -oN nmap.txt
```
It is noticable that open ports are `22`, `80` ,`8080`.
Accessing the target website via port `80`, it is obvious to garsp the `username : scr1ptkiddy` and a page call `silverpeas` in the contact section
The process of enumerating paths of the target with to ports `80` and `8080` was executed by `gobuster`
```
gobuster dir -u "http://10.10.9.219" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js,.txt -t 100
```
- With port `80`:
`/assets`
`/images`
`/README.txt`
`/LICENSE.txt`
```
gobuster dir -u "http://10.10.9.219:8080" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js,.txt -t 100
```
- With port `8080` (cannot access normally to this port):
`/console`
`/website`
Manually brute-forcing path `silverpeas` to 2 ports and port `8080` did respond and redirect to a login page.

