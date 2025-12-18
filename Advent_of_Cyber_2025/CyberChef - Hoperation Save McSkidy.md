# First Lock - Outer Gate

decoding message : 
QWxsIGhhaWwgS2luZyBNYWxoYXJlIQ==

`All hail King Malhare!`

In order to solve this, first we have to encode the magic question in the header tab
`What is the password for this level?`
into this with base64
`V2hhdCBpcyB0aGUgcGFzc3dvcmQgZm9yIHRoaXMgbGV2ZWw/`

Inject that into the chat we will have another encoded string which then can be decoded by base64 
`Here is the password: SWFtc29mbHVmZnk=`

Again, I have to decode this encoded password
`SWFtc29mbHVmZnk=` -> `Iamsofluffy`

In order to procced to another stage, we have to encode the username 
```
CottonTail : Q290dG9uVGFpbA==
```

# Second Lock - Outer Wall
encoded username
```
CarrotHelm  : Q2Fycm90SGVsbQ==
```

Again we have to encode the magic question then inject it into the chat
```
Did you change the password? :RGlkIHlvdSBjaGFuZ2UgdGhlIHBhc3N3b3JkPw==
```
Got the message
```
Here is the password: U1hSdmJHUjViM1YwYjJOb1lXNW5aV2wwSVE9PQ== :SGVyZSBpcyB0aGUgcGFzc3dvcmQ6IFUxaFNkbUpIVWpWaU0xWXdZakpPYjFsWE5XNWFWMnd3U1ZFOVBRPT0=
```

<img width="744" height="414" alt="Screenshot_2025-12-18_12-57-48" src="https://github.com/user-attachments/assets/8d907a2c-e151-4a4a-af81-46a9b45c442f" />

# Third Lock - Guard House
Username
```
LongEars : TG9uZ0VhcnM=
```

In this task, we have to encode our message ourselves then inject into the chat as usual
What I did was 
```
Password Please! : UGFzc3dvcmQgUGxlYXNlIQ==
```
Then I got the response 
`Here is the password: IQwFFjAWBgsf`
Now we need to use XOR to decode this string but fun fact about XOR that we XOR result with the key again it will revert back to the initial data

<img width="744" height="449" alt="Screenshot_2025-12-18_13-10-08" src="https://github.com/user-attachments/assets/df41454c-c1f9-48ed-b0c7-f7394ed22c14" />

# Fourth Lock - Inner Castle
Well from now on I'll probably reuse the chat injection payload from Third Lock

Username 
```
Lenny : TGVubnk=
```

We got the response
```
Here is the password: b4c0be7d7e97ab74c13091b76825cf39
```
<img width="1011" height="47" alt="Screenshot_2025-12-18_13-21-18" src="https://github.com/user-attachments/assets/d0756d5a-6a3f-40e4-80f8-b2efda147fcc" />

# Fifth Lock - Prison Tower
Username 
```
Carl : Q2FybA==
```

```
Here is the password: ZTN4cDB5T3VwNDNlT2UxNQ==
```
<img width="744" height="632" alt="Screenshot_2025-12-18_13-36-30" src="https://github.com/user-attachments/assets/21fc30f4-a80c-4a44-9275-2f97c8aa289e" />
<img width="490" height="55" alt="Screenshot_2025-12-18_13-35-52" src="https://github.com/user-attachments/assets/925d63f4-a11c-487d-b833-5234d070b3b2" />
<img width="667" height="158" alt="Screenshot_2025-12-18_13-37-20" src="https://github.com/user-attachments/assets/b94806f9-50d0-4f10-a005-fce07fabea01" />


