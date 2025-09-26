# Overview 
<img width="715" height="145" alt="Screenshot_2025-09-26_21-51-56" src="https://github.com/user-attachments/assets/3e8aa7e4-9bf9-484e-8050-3d21f60a3764" />
The description also provide me with a hint to use needed tools like `nmap` and `nc`

# Scanning
I kick off the scanning process with this command 
```
map -T4 -vvv -A -O -sV 10.201.123.162
...
PORT      STATE SERVICE REASON         VERSION
22/tcp    open  ssh     syn-ack ttl 60 OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 7d:dc:eb:90:e4:af:33:d9:9f:0b:21:9a:fc:d5:77:f2 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDLgmREvZByFxoWZEVRzGogcBi04sMOoMHUcJ8P2t9mi8q1V+lRvZczumOJ9RliQ1khNRHsIgnJ7/Yp4iCz9UFRLYsIlQ2PAgODBjNyhIWepELlSatGLZqviC6qnxEASC2OrwFvHmYNklZIjbc4+Yli2bn8I2PjntRKoMQHjehoWip/V955dYwa7KysnPPMsmXlrx9dhHJ0yheqYIOxwlN2xRC7ZtQMfY28mn0ifRhMnM9RRgC5CDAJh3QeJYM90gBdIq466x4p6yWQzjK/t0i5zIKFBJBLp85DaT3WkWBn3ueHFoedeeAsXeO16BNX6SomLNWsslp5a3E1YQiOBX3LNEm64Rr8ekPxtrXhTzcBUMGJYzc7QaE2/N9yCnkvL0vCApO+XFmo+lr9bAt8qGixpqk1awPqIhxOTUWIr4lAShsu7b+mwQlGeYO7aEeaO3R7ZFaZwGfp2vF/yYwReT+mVimFbs67yiVcMCNhVgvI5vLFrh+OyvufUK2tnCe4xbs=
|   256 83:a7:4a:61:ef:93:a3:57:1a:57:38:5c:48:2a:eb:16 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBEuffg4vybQqbP0RmSCYjd6nillwqgiGF1OAIPUsBCewLy6eiVYsnzAF3Z2TAHKjA8GqTlhRu05KWfdys7J+MQc=
|   256 30:bf:ef:94:08:86:07:00:f7:fc:df:e8:ed:fe:07:af (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILHnXOBOqSHpnx9JPguwJAsiurhjCuLjkOIfNSp9f9ai
2222/tcp  open  ssh     syn-ack ttl 61 OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 60:87:1a:84:b2:be:88:8d:3e:f7:1e:f6:7a:1b:02:a2 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDYt8usO7N+o9h3Z393drIGkHqTa63wtn2PoUHhLzkRIf0idwBWV6f47B701k2PzYK7WPTcoV1dNgNnsTMlDvtPUsWC6ny4aBM/TVOzU71Q1BoZIfVQqukksXm85Hm8QXNKh96LLV6mLmhmk75F7g9ISdHreF1JCYCOP3vPSG9fvWbqPELyN8zmFtS3i+EvFW7YoUf9deZfCa505GokjEOV8vB7R9pvvUq5RfkpSg6ol2F8bVwQY3xbESm92w/oTZxcJ6oa/kO23O8iyI6wHK50ESQ3ePs58Uv3ZtsVg3fCsJTuC+I2pwNP3SBJ1fst4GdELLx8PBPmgOPchs/H0k3VD/n/z0v0NKyQ//RPrkFnu+FNSSwixfTIn4Zo1MWY0f03FvbzkN+eovEJsFi8ynmkeq1Vu4USifsfx7nA+6FTvQ3GB/Ur0Xglh95CShnh2IOb0Sbt18IUx0QPGSk+QrHcLesnqgetQlTfpFuEdPetkBkNsIiNiQvot4jWEbMXv5c=
|   256 d0:8f:c4:61:cd:09:b8:69:2b:85:cc:6d:1d:9e:7c:e5 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBFTm8FuQEKyip01bo6kY1VIGybDGM6l+tTw8I2d4HhMPlEcGJdIiNXCFeYxPz4BZAIp7rlneZ+EWuEABqjxisbg=
|   256 2a:22:a0:d7:fc:79:54:a6:be:8f:67:fd:13:09:2d:57 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGmndVjSW/pF2WcmI7hkYq5trxEKkLsYFamiFiY0kLhy
31337/tcp open  Elite?  syn-ack ttl 60
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, Kerberos, LANDesk-RC, LDAPBindReq, LDAPSearchReq, LPDString, NULL, RPCCheck, RTSPRequest, SIPOptions, SMBProgNeg, SSLSessionReq, TLSSessionReq, TerminalServer, TerminalServerCookie, X11Probe: 
|     In case I forget - user:pass
|_    ubuntu:Dafdas!!/str0ng
```
After the scan I got 2 credentials, `user:pass` and `ubuntu:Dafdas!!/str0ng` and I know that port 22 is open, so I decide to use `ssh` instead of `nc`
```
ssh user@10.201.123.162
$ ls
flag.txt
$ cat flag.txt
```
DOne~~~ Happy Hacking!!!


