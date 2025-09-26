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











