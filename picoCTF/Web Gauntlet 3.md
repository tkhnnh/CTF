<img width="1919" height="595" alt="Screenshot_2025-09-29_23-48-46" src="https://github.com/user-attachments/assets/5e79fd1f-7918-4d0f-b570-ea5f2df04a3b" />
I failed with that payload and kinda cluless at that time. But I found that the operator `GLOB` would be useful to exploit this site

```
username: adm'||'in
password: ' GLOB '*
```
Explanation why would I use the operator `GLOB`
`GLOB '*'` is a pattern that would match any file or string, effectively "globbing" or finding everything.
<img width="1920" height="569" alt="Screenshot_2025-09-29_23-57-46" src="https://github.com/user-attachments/assets/066d8302-c4de-4d2c-a20f-9102f763ba5e" />
![Uploading Screenshot_2025-09-30_22-45-27.png…]()

It's time to get the flag 
Happy hacking!!!
