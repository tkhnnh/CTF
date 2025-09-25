# Overview 
The target is a normal homepage
<img width="1920" height="936" alt="Screenshot_2025-09-25_22-22-02" src="https://github.com/user-attachments/assets/5c78df0b-0a8b-4f1e-92a1-e48b403ede4f" />

# Action
Carefully observe the hint, I can easily notie that where the flag is `folders folders folders` which should look like this `/folders/folders/folders`
so observe the page source I found that there is a folder called `/secret`
<img width="678" height="51" alt="Screenshot_2025-09-25_22-24-35" src="https://github.com/user-attachments/assets/70d2a6b8-76f5-4835-a4e6-68e47c83dd36" />

Then I use `gobuster`  to find the second folder. Even if I did not check the page source. I would use this command to find `/secret`
```
gobuster dir -u "http://saturn.picoctf.net:63342/" -w /usr/share/wordlists/dirb/common.txt -t 64
/index.html           (Status: 200) [Size: 1023]
/secret               (Status: 301) [Size: 169] [--> http://saturn.picoctf.net/secret/]
```
Then continuing enumerate endpoints
```
obuster dir -u "http://saturn.picoctf.net:63342/secret/" -w /usr/share/wordlists/dirb/common.txt -t 64
/assets               (Status: 301) [Size: 169] [--> http://saturn.picoctf.net/secret/assets/]
/hidden               (Status: 301) [Size: 169] [--> http://saturn.picoctf.net/secret/hidden/]
/index.html           (Status: 200) [Size: 468]
```
<img width="1920" height="931" alt="Screenshot_2025-09-25_22-29-31" src="https://github.com/user-attachments/assets/ccc6b9a9-15bb-482b-992a-4e0ac210bedc" />

Yeah it said that I almost and implied that I am close enough so continue
<img width="1920" height="929" alt="Screenshot_2025-09-25_22-32-13" src="https://github.com/user-attachments/assets/e47c01a4-5910-447e-9b8a-9fcbcb4701d5" />
```
gobuster dir -u "http://saturn.picoctf.net:63342/secret/hidden/" -w /usr/share/wordlists/dirb/common.txt -t 64
```
for this folder I could not use `gobuster` to find it but I found the next folder in the page source
<img width="438" height="53" alt="Screenshot_2025-09-25_22-32-58" src="https://github.com/user-attachments/assets/74332c22-e7dc-47bf-a4f3-aaeb87bbb543" />

Finally
<img width="1920" height="291" alt="Screenshot_2025-09-25_22-37-09" src="https://github.com/user-attachments/assets/ff1fc8d8-95cc-4321-96d1-213ca692cfaa" />
Done
<img width="700" height="261" alt="Screenshot_2025-09-25_22-37-19" src="https://github.com/user-attachments/assets/dbc79d0d-9e3b-4695-a7cb-4877316731c7" />

Happy Hacking!!!
