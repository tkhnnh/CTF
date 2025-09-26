# Overview
It is the same website as the room `Search Source` iykuk

# Actions
First I need to find some hidden directories via `gobuster`
```
gobuster dir -u "http://saturn.picoctf.net:50823/" -w /usr/share/wordlists/dirb/common.txt -t 64

/css                  (Status: 301) [Size: 169] [--> http://saturn.picoctf.net/css/]
/images               (Status: 301) [Size: 169] [--> http://saturn.picoctf.net/images/]
/index.html           (Status: 200) [Size: 15920]
/js                   (Status: 301) [Size: 169] [--> http://saturn.picoctf.net/js/]
/robots.txt           (Status: 200) [Size: 184]
```
I found `robots.txt` file which my elaborate with useful information
<img width="587" height="220" alt="Screenshot_2025-09-26_15-14-48" src="https://github.com/user-attachments/assets/a94eb366-bf00-441a-b51e-c8c2ecb30b36" />

I used `cyberchef` to decipher three encoded lines in the `robots.txt` file
<img width="1382" height="573" alt="Screenshot_2025-09-26_15-16-46" src="https://github.com/user-attachments/assets/3ac228b5-1446-47d6-840f-ba75bc8f127a" />
<img width="1005" height="562" alt="Screenshot_2025-09-26_15-17-05" src="https://github.com/user-attachments/assets/1a84c890-aff0-4027-8f71-0d8cc34186bd" />
<img width="847" height="587" alt="Screenshot_2025-09-26_15-17-53" src="https://github.com/user-attachments/assets/4c0833b3-bd69-4c37-ab9b-9f1ac6b0d2b4" />

Well, the flag is located in the `/js/myfile.txt`
<img width="627" height="165" alt="Screenshot_2025-09-26_15-14-26" src="https://github.com/user-attachments/assets/d44843fc-d610-4e61-9ecd-7b27b3782572" />
