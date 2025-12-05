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
