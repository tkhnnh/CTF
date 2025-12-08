# Discovering Exposed Services

## Scan the whole range

```
 nmap -p- --script=banner MACHINE_IP

```
`--script=banner` switch -> perform banner grabbing, a technique for identifying service versions by capturing text banners they send when a connection is established.

using 
```
ftp MACHIINE_IP PORT
```
if there is FTP service, although the port number for FTP is usually 21, able to switch to any number.


## Port scan modes

Attempting to establish connection with `nc`

## TCP and UDP
Not only does TCP connection contains hidden secret, UDP also has.

Establishing scanning UDP connection with `-sU` switch within `nmap` tool

```
dig @MACHINE_IP TXT DOMAIN +short
```
`TXT` -> format
`DOMAIN` -> where to dig up the information
`+short` -> just output essential information

# On-Host Discovery
## List Listening Ports
```
ss -tunlp
```

What evil message do you see on top of the website?
<img width="1920" height="220" alt="Screenshot_2025-12-09_01-13-07" src="https://github.com/user-attachments/assets/f9dcd6c0-3679-43dc-9b5d-be4f963e860d" />

What is the first key part found on the FTP server?
<img width="598" height="201" alt="Screenshot_2025-12-09_01-07-01" src="https://github.com/user-attachments/assets/743aa709-2c22-4a91-9078-4144bc741a9d" />

What is the second key part found in the TBFC app?
<img width="487" height="200" alt="Screenshot_2025-12-09_01-08-24" src="https://github.com/user-attachments/assets/ab64d88c-6bde-4481-a007-872a22ef9caf" />


What is the third key part found in the DNS records?
<img width="401" height="72" alt="Screenshot_2025-12-09_01-11-00" src="https://github.com/user-attachments/assets/cf4d222f-9576-4922-a484-db323fb00573" />


Which port was the MySQL database running on?
`3306`

Finally, what's the flag you found in the database?
<img width="1483" height="393" alt="Screenshot_2025-12-09_01-17-13" src="https://github.com/user-attachments/assets/c642b57a-bc0b-4bdb-84f0-6d18c6432d99" />


Happy Hacking!!!!
