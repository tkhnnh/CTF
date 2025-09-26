# Overview
The target is just a normal yoga advertising page, and the page source is inlucding a lot of files. Having a look at the hint tells me what I need to do.
So I am going to copy (mirror) the whole source code of the site into my machine in order to get the flag

```
wget -mpEk {ip}
```
the I got the folder called `/saturn.picoctf.net:49503`, I just access that directory and use this command
```
grep -r picoCTF
```
<img width="940" height="656" alt="Screenshot_2025-09-26_14-37-19" src="https://github.com/user-attachments/assets/e3041edb-c291-4778-9079-b97fee35d312" />

then I got the flag
Happy Hacking!!!
