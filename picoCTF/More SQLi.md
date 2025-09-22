# Overview
The target is a login portal. 

<img width="410" height="307" alt="Screenshot_2025-09-22_13-56-03" src="https://github.com/user-attachments/assets/8c068ed0-b081-4909-b07e-f066e37c8451" />

Whenever I enter the password and username, the site will response like this
<img width="613" height="80" alt="Screenshot_2025-09-22_13-56-35" src="https://github.com/user-attachments/assets/0b5153bf-0d51-4f27-98b1-8b1272735686" />

# Payload
So what I need to do is find the logic behind the password input field and break it. This is the first payload that I am going to use
```
' or 1==1;--
```
and it worked. Let's breakdown the concept. So if I enter the username and password wrong, the site exposes the query to database to me.
```text
SQL query: SELECT id FROM users WHERE password = '' AND username = ''
```
The password is placed first, so the payload to break this is to close the input using `'` and add 1==1 which is always true, then how to deal with variable `username` using comment to ignore it then complete the payload.
I got in but where is the flag? Am I supposed to mess with search bar
<img width="1090" height="728" alt="Screenshot_2025-09-22_21-46-41" src="https://github.com/user-attachments/assets/57a18442-abd2-47c5-90bc-38d7b13f22b2" />

So I have to use BurpSuite to check whether I made a mistake somehow but I got the flag. I realised after I successfully login into the site, it redirects me to the search box section without my awareness
<img width="1260" height="683" alt="Screenshot_2025-09-22_21-53-06" src="https://github.com/user-attachments/assets/1b016db5-201c-4d2d-8dc3-13ee3bf8ef2d" />
Happy hacking!!!!
