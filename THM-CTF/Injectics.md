# Recon
Using this command 
```
nmap -sV -T4 -O -A 10.49.171.59
....
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 5f:dc:93:8d:01:8b:9b:ed:2c:65:da:62:88:55:aa:ea (RSA)
|   256 a7:30:a0:15:63:cb:37:d5:bd:f7:10:1a:47:71:18:41 (ECDSA)
|_  256 22:8b:1a:ad:0c:1e:73:ec:dd:43:92:5a:8f:ef:91:50 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: Injectics Leaderboard
|_http-server-header: Apache/2.4.41 (Ubuntu)
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.15
OS details: Linux 4.15
Network Distance: 3 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
...
```

So I have port 22 and port 80 are open which inform me that I can access the webpage of the target laterally
Next it is time for the directory enumeration with the following command

```
gobuster dir -u "http://10.49.171.59/" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 64

...
/flags                (Status: 301) [Size: 312] [--> http://10.49.171.59/flags/]
/css                  (Status: 301) [Size: 310] [--> http://10.49.171.59/css/]
/js                   (Status: 301) [Size: 309] [--> http://10.49.171.59/js/]
/javascript           (Status: 301) [Size: 317] [--> http://10.49.171.59/javascript/]
/vendor               (Status: 301) [Size: 313] [--> http://10.49.171.59/vendor/]
/phpmyadmin           (Status: 301) [Size: 317] [--> http://10.49.171.59/phpmyadmin/]
/server-status        (Status: 403) [Size: 277]
...
```

Look like I have 2 interesting directories right here `/flags` and `/vendor`. Let's enumerate to find their hidden secret folders if possible

`/vendor` first
```
fuf -u http://10.49.171.59/vendor/FUZZ -w /usr/share/wordlists/dirb/big.txt
....
.htaccess               [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 217ms]
.htpasswd               [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 216ms]
bin                     [Status: 301, Size: 317, Words: 20, Lines: 10, Duration: 229ms]
composer                [Status: 301, Size: 322, Words: 20, Lines: 10, Duration: 196ms]
symfony                 [Status: 301, Size: 321, Words: 20, Lines: 10, Duration: 206ms]
twig                    [Status: 301, Size: 318, Words: 20, Lines: 10, Duration: 202ms]
....
```

Secondly, `/flags`
```
ffuf -u http://10.49.171.59/flags/FUZZ -w /usr/share/wordlists/dirb/big.txt
...
.htaccess               [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 212ms]
.htpasswd               [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 5132ms]
```
Nothing much for this one I guess. However, I still got something inside `/vendor`
Diving more inside `/vendor/composer`

```
ffuf -u http://10.49.171.59/vendor/composer/FUZZ -w /usr/share/wordlists/dirb/big.txt
...
.htaccess               [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 1095ms]
.htpasswd               [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 3102ms]
LICENSE                 [Status: 200, Size: 1070, Words: 153, Lines: 22, Duration: 216ms]
```

`/vendor/twig`
```
ffuf -u http://10.49.171.59/vendor/twig/FUZZ -w /usr/share/wordlists/dirb/big.txt
...
.htpasswd               [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 206ms]
.htaccess               [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 4354ms]
twig                    [Status: 301, Size: 323, Words: 20, Lines: 10, Duration: 178ms]

```

For mroe information about `twig` -> Twig is a PHP template engine used to generate dynamic HTML for web applications by separating presentation (HTML) from application logic.
Keep doing recursively into another `/twig`

```
ffuf -u http://10.49.171.59/vendor/twig/twig/FUZZ -w /usr/share/wordlists/dirb/big.txt
...
.htaccess               [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 3572ms]
.htpasswd               [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 4597ms]
LICENSE                 [Status: 200, Size: 1513, Words: 244, Lines: 28, Duration: 205ms]
doc                     [Status: 301, Size: 327, Words: 20, Lines: 10, Duration: 184ms]
lib                     [Status: 301, Size: 327, Words: 20, Lines: 10, Duration: 209ms]
src                     [Status: 301, Size: 327, Words: 20, Lines: 10, Duration: 202ms]
```

Inside `doc` 
```
.htpasswd               [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 408ms]
.htaccess               [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 4503ms]
filters                 [Status: 301, Size: 335, Words: 20, Lines: 10, Duration: 205ms]
functions               [Status: 301, Size: 337, Words: 20, Lines: 10, Duration: 202ms]
tags                    [Status: 301, Size: 332, Words: 20, Lines: 10, Duration: 210ms]
tests                   [Status: 301, Size: 333, Words: 20, Lines: 10, Duration: 204ms]
```

Nothing left for me to enumerate. Let's have a look at the http page and found interestingly exposed information
<img width="588" height="206" alt="Screenshot_2025-11-30_01-06-38" src="https://github.com/user-attachments/assets/7923104e-d859-4e1d-b58b-d58a91963332" />

Well, the developer team is actually trolling by exposing testing credentials
<img width="1917" height="467" alt="Screenshot_2025-11-30_01-00-54" src="https://github.com/user-attachments/assets/dd7f5594-9c1f-40cc-94fa-f904e3b28a26" />
