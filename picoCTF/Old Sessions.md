URL : [Old Sessions](https://play.picoctf.org/practice/challenge/739?category=1&difficulty=1&page=1)

First I check that whether the username `admin` has been used for special account or not
<img width="592" height="74" alt="Screenshot 2026-04-28 001739" src="https://github.com/user-attachments/assets/5980fe92-29c5-4d0a-a76c-83ea9a0dd73f" />

So I try to create a test account and see what are inside when logged in. I noticed there is an endpoint called `/sessions`
<img width="1917" height="761" alt="Screenshot 2026-04-28 001650" src="https://github.com/user-attachments/assets/2874ecf7-a701-4a20-876f-1c9b2792db95" />

And inside `/sessions` endpoint I found the session for the `Admin` user which can help me escalate the privilege
<img width="1918" height="183" alt="Screenshot 2026-04-28 001954" src="https://github.com/user-attachments/assets/82b6b6e6-4b62-4944-aff3-e5b86ac1e122" />

By changing the sessions help me become `admin` user 
<img width="1258" height="556" alt="Screenshot 2026-04-28 002123" src="https://github.com/user-attachments/assets/22c9232f-e96a-40f9-bd95-d602f6adbcfa" />


Happy Hacking !#!@#!#1231
