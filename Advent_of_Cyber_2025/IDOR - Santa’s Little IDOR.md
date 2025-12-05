# IDOR stands for Insecure Direct Object Reference
Access control vulnerability
Web applications often use references to determine what data to return when making up a reqquest
Since the web application isn't verifying that ther person making the request is the same as the one who own the package

# Identity Defines Our Reach
- Authentication -> after logging receive cookie or token called session information
- Authorization -> IDOR does not require authentication -> fix authentication first before fixing the authorization issue

Privilege Escalation Types:
- Vertical Privilege Escalation -> gain access to more features
e.g normal user but can perform actions that should be restricted for an administrator
- Horizontal privilege escalation -> use a feature which are authorized to use, but gain access to data that not allowed to access
e.g see my own account, not other's.

What does IDOR stand for?
`Insecure Direct Object Reference`

What type of privilege escalation are most IDOR cases?
`Horizontal`

Exploiting the IDOR found in the view_accounts parameter, what is the user_id of the parent that has 10 children?
`15`
<img width="1745" height="958" alt="Screenshot_2025-12-06_09-21-23" src="https://github.com/user-attachments/assets/ee06e9cb-8dc6-4423-8a9c-5709ce9f8c65" />
<img width="1598" height="901" alt="Screenshot_2025-12-06_09-21-33" src="https://github.com/user-attachments/assets/e3988f62-3b9d-4d4a-8fac-1a929204fd26" />

Bonus Task: If you want to dive even deeper, use either the base64 or md5 child endpoint and try to find the id_number of the child born on 2019-04-17? To make the iteration faster, consider using something like Burp's Intruder. If you want to check your answer, click the hint on the question.
`19`

Command code to generate a list of encoded base64 numbers ranging from 1-30
```
for i in {1..30}; do
  echo -n "$i" | base64
done
```
<img width="1742" height="964" alt="Screenshot_2025-12-06_09-35-28" src="https://github.com/user-attachments/assets/eb03d5e6-fded-4ae7-8933-df6e83355808" />
<img width="1598" height="903" alt="Screenshot_2025-12-06_09-35-39" src="https://github.com/user-attachments/assets/da983ada-e5e0-46a1-9f33-68319d89998f" />

Bonus Task: Want to go even further? Using the /parents/vouchers/claim endpoint, find the voucher that is valid on 20 November 2025. Insider information tells you that the voucher was generated exactly on the minute somewhere between 20:00 - 24:00 UTC that day. What is the voucher code? If you want to check your answer, click the hint on the question.
