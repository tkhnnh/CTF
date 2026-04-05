## Link
[Data Vault](https://app.hackinghub.io/hubs/snyk-ftf-26-data-vault)


This is the website
<img width="1917" height="767" alt="Screenshot 2026-04-05 232048" src="https://github.com/user-attachments/assets/83f47d9c-6e6c-44f7-99b6-34541ba02b74" />

So I captured the POST request and attempting to modify the `Content-Type` from `application/json` -> `application/xml`
<img width="1258" height="382" alt="Screenshot 2026-04-05 232251" src="https://github.com/user-attachments/assets/51e89587-945e-4fe1-bbf5-210289660cd6" />

Then I deduced that site is vulnerable to XXE so not only I modify the `Content-Type` header only but also change the format of the payload in order to match with Header
from 

```
{"name":"tester","bio":"im just a random guy\n"}
```
into 
```
<?xml version="1.0" encoding="UTF-8"?><foo>bar</foo>
```

And it works
<img width="1261" height="376" alt="Screenshot 2026-04-05 232713" src="https://github.com/user-attachments/assets/8277f48a-1bce-4970-b873-784f9c7d2a93" />

By adding `DOCTYPE` into payload and using referenced entity pointed to /etc/passwd 
<img width="1255" height="521" alt="Screenshot 2026-04-05 234152" src="https://github.com/user-attachments/assets/2d4cd7f8-f4fd-4613-a369-a9b41628781d" />


Happy Hackinggg!!!!!

# Reference 
[What is XXE](https://portswigger.net/web-security/xxe)
