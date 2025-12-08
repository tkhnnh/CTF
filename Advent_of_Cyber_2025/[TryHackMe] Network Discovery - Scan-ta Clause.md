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
