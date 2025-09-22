# What is No SQL injection 
A vulnerability that allow attacker to tamper with the queries sent to NoSQL database

In this case, I am going to use it for Bypassing authentication or protection mechanisms.

# Overview
The target is a login page 
<img width="1920" height="928" alt="Screenshot_2025-09-22_09-46-15" src="https://github.com/user-attachments/assets/2d6862a0-38d7-45aa-8b99-91badba7f090" />

The room also provide me with the source code in the zip file
```
gunzip app.tar.gz
tar -xvf app.tar
```
Now I have a folder called `app` with many 4 files inside. However, I had a look at the file `server.js` which give me the email
<img width="714" height="274" alt="Screenshot_2025-09-22_09-51-55" src="https://github.com/user-attachments/assets/a1dd0171-1a73-4be7-a161-e99eb8152fab" />

`picoplayer355@picoctf.org`
Next I need to intercept the process of POST of the login form
<img width="1227" height="460" alt="Screenshot_2025-09-22_09-57-10" src="https://github.com/user-attachments/assets/39d3e2fd-d08e-493f-a097-cfec92322098" />
But right now, I have limited knowledge of what is the database server for this one, so I further examine backend server file called `package.json`

<img width="495" height="304" alt="Screenshot_2025-09-22_10-01-54" src="https://github.com/user-attachments/assets/ff676d67-026d-475d-af01-ed0b0e9c49b1" />

And then I carefully examine the `server.js` again and find that the fields input for both username and password start and end with `{` `}` which is weird
<img width="603" height="173" alt="Screenshot_2025-09-22_10-01-31" src="https://github.com/user-attachments/assets/14f25d3f-3797-4a34-a7ad-d4814e5df043" />

I search up for payloads and found this repo only contains mongodb nosql payload. It is like a txt file, which I need to narrow down the payload having the format start and end with `{}`. The linke to the repo is [here](https://github.com/cr0hn/nosqlinjection_wordlists/blob/master/mongodb_nosqli.txt#L6C1-L6C11)

Then I got a pair
```
{"username": {"$ne": null}, "password": {"$ne": null}}
```
<img width="1228" height="674" alt="Screenshot_2025-09-22_10-09-01" src="https://github.com/user-attachments/assets/01bb4700-89e1-4e4d-89cc-72cf18da26b0" />

But where is the flag? It definitely is the token so using `cyberchef` to decipher the token.
<img width="1536" height="828" alt="Screenshot_2025-09-22_10-14-14" src="https://github.com/user-attachments/assets/5e35f374-083b-4437-b02e-b00dcbcab877" />

I got the flag. My misison completed successfully. Happy hacking!!

# Reference
[Concept of NoSQL Injection](https://portswigger.net/web-security/nosql-injection)
[payloads for mongoDB](https://github.com/cr0hn/nosqlinjection_wordlists/blob/master/mongodb_nosqli.txt#L6C1-L6C11)
