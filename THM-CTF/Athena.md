# Link
[Athena](https://tryhackme.com/room/4th3n4)

# Reconnaisance
Starting this phase with nmap tcp scanning to check all the ports
```
$ nmap -sV -T5 -vv -O -A 10.49.182.150
```

```
PORT    STATE SERVICE     REASON         VERSION
22/tcp  open  ssh         syn-ack ttl 62 OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3b:c8:f8:13:e0:cb:42:60:0d:f6:4c:dc:55:d8:3b:ed (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCqrhWpCkIWorEVg4w8mfia/rsblIvsmSU9y9mEBby77pooZXLBYMvMC0aiaJvWIgPVOXrHTh9IstAF6s9Tpjx+iV+Me2XdvUyGPmzAlbEJRO4gnNYieBya/0TyMmw0QT/PO8gu/behXQ9R6yCjiw9vmsV+99SiCeuIHssGoLtvTwXE2i8kxqr5S0atmBiDkIqlp+qD1WZzc8YP5OU0CIN5F9ytZOVqO9oiGRgI6CP4TwNQwBLU2zRBmUmtbV9FRQyObrB1zCYcEZcKNPzasXHgRkfYMK9OMmUBhi/Hveei3BNtdaWARN9x30O488BmdET3iaTt5gcIgHfAO+5WzUPBswerbcOHp2798DXkuVpsklS9Zi9dvpxoyZFsmu1RoklPWea+rxq09KRjciXNvy+jV8zBGCGKwwi62nL9mRyA5ZakJKrpWCPffnEMK37SHL0WqWMRZI4Bbj2cOpJztJ+5Ttbj5wixecnvZu8hkknfMSVwPM8RqwQuXtes8AqF6gs=
|   256 1f:42:e1:c3:a5:17:2a:38:69:3e:9b:73:6d:cd:56:33 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBPBg1Oa6gqrvB/IQQ1EmM1p5o443v5y1zDwXMLkd9oUfYsraZqddzwe2CoYZD3/oTs/YjF84bDqeA+ILx7x5zdQ=
|   256 7a:67:59:8d:37:c5:67:29:e8:53:e8:1e:df:b0:c7:1e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBaJ6imGGkCETvb1JN5TUcfj+AWLbVei52kD/nuGSHGF
80/tcp  open  http        syn-ack ttl 62 Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-methods: 
|_  Supported Methods: HEAD GET POST OPTIONS
|_http-title: Athena - Gods of olympus
139/tcp open  netbios-ssn syn-ack ttl 62 Samba smbd 4
445/tcp open  netbios-ssn syn-ack ttl 62 Samba smbd 4
```
There are 4 open ports

Let's focus on enumerating port 445 and 139 which are Samba service by using `smbmap` and `smbclient`
First, using `smbmap` for checking shares in the SMB service
```
smbmap -H 10.49.182.150
```

<img width="941" height="157" alt="Screenshot 2026-02-23 233829" src="https://github.com/user-attachments/assets/738f6545-ce64-4b9a-a8bb-4014bfe9467e" />

Then using `smbclient` to connect and get the file `msg_for_administrator.txt`
<img width="953" height="248" alt="Screenshot 2026-02-23 233846" src="https://github.com/user-attachments/assets/5b3a3f94-87cd-4951-8377-a6692332d53c" />

Here is the content of the file
<img width="944" height="203" alt="Screenshot 2026-02-23 233859" src="https://github.com/user-attachments/assets/2e3f9a6d-ad85-4455-befb-03d6a7af3c7e" />

# Exploitation
There is an endpoint service which is called `/myrouterpanel` allows user to ping devices providing IP address to check network connectivity

Attempting to input special character like 
```
&
&&
||
|
;
```
Leading to be flagged as `Attempt Hacking!`
<img width="1916" height="803" alt="Screenshot 2026-02-23 231504" src="https://github.com/user-attachments/assets/3f4fbfee-cb7d-40bd-a3e9-2e39dac1d591" />

<img width="192" height="45" alt="Screenshot 2026-02-23 231517" src="https://github.com/user-attachments/assets/45fea67d-8013-427f-b06e-526fb53c580a" />

After a while checking, I found the way to bypass the filter by combining like this
<img width="1919" height="730" alt="Screenshot 2026-02-23 233735" src="https://github.com/user-attachments/assets/ee8bee1b-0346-4854-ae15-f1fd7a3304b0" />
<img width="540" height="72" alt="Screenshot 2026-02-23 233751" src="https://github.com/user-attachments/assets/f553dadf-7e7b-471a-b675-35f67cfccedf" />



