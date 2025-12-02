# Recon
Start with `nmap`

```
nmap -sV -T4 -vv -O -A 10.48.188.141
...
PORT      STATE SERVICE  REASON         VERSION
22/tcp    open  ssh      syn-ack ttl 62 OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 a9:8d:f7:d4:1d:5e:e1:b8:1d:57:20:d9:b9:d1:fc:af (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDPV6+VGL/iHyamzb6KCdP3NaZPRKHme75lOD0o9/sAGmI1HxA6mcLAGin/vxAVYpkXzS6tteT859RQLmmKLQR1c6BlFYCaAmpsJx/EVfn6UEOvuvbkj+H6g4VFKUqOqquyfha5XoYhattRGIMVvqwh7HoX2mcFFTk8RkibAa6dprIMqGy515xUW5rX3hb7wJmZO7hDHcecwp0hPPVkhXxAZ9YJCNfiUGqMJk3L69mjISYRyItthpZ7LGBzuaR9iZAlGnmRjkPVePZFCuHV/TAIc+9CKvB9M7vJeoCnlfxwT8PKLeWCx6GOZJtF9zyvLauvO0+qdpIuVo2R1we+bytXdJk2f1U8xS56S8z04CwtzLjmanqUZhObl6jB8xoWewKznr9rEkHViPsbUHWtBcRUo/paD2rovEhAQCv19DAbwtYZUY6ESqn1BhwdFjELEs/aVx6fclr2a87ZmAUrv56OudigTH3qXnqk5cg/2ScPIoiuwwHM2Cp0Meuc+BF5TsE=
|   256 c7:8c:44:56:5f:c4:84:b5:9c:6a:5c:ce:b4:17:ff:bd (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBIAROIlTnVU6gXemJiwjdG/atKsX+o2fMBicOsoHWGEyN5BqetuEqS+9+mb9AXgS/dHKXumHtHYXQ3ofLVNHtpY=
|   256 5b:b3:31:f6:05:f2:48:14:b2:dc:5e:de:59:82:cc:33 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEqE1/aev3VMLhm9ke9hke6TvAka9yotiXalqfJ3ZRI+
25/tcp    open  smtp     syn-ack ttl 62 Postfix smtpd
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Issuer: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2021-11-10T16:53:34
| Not valid after:  2031-11-08T16:53:34
| MD5:   05c8:4559:9811:a54f:9c53:b3ee:f6ad:f0fd
| SHA-1: a24d:7a7f:9ac1:8045:5c5f:5b7c:721a:4e21:0599:ed7c
| -----BEGIN CERTIFICATE-----
| MIIDOTCCAiGgAwIBAgIUZOpVp2fjesLBhoJHfQXOvrRFh2AwDQYJKoZIhvcNAQEL
| BQAwNDEyMDAGA1UEAwwpaXAtMTAtMTAtMzEtODIuZXUtd2VzdC0xLmNvbXB1dGUu
| aW50ZXJuYWwwHhcNMjExMTEwMTY1MzM0WhcNMzExMTA4MTY1MzM0WjA0MTIwMAYD
| VQQDDClpcC0xMC0xMC0zMS04Mi5ldS13ZXN0LTEuY29tcHV0ZS5pbnRlcm5hbDCC
| ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANCGzRe8Ucyrg1ICrmylNf/G
| Dhe8gGUJsnSdBBwzEhofznOXvjGJ/P+5/ScXSNm5Bb632xtPcZ3wSq9xHq8JqZMu
| oXjoyd4U6VK6aV4xjxlwdE33DgsAHXORv9PkMi+NeYDFsJrdRznSV64mc9xhIqSk
| WdnALkBXvNZcTwy6feITP3F4YTGa5ewRNJSVutU4hBY1CfroZxRnff6kkbF0iqQc
| dSHPjK3NeAZnp4iVID8rBuV/fjjOtZ53z1u6cXmQVc2fljvD4GN3TxV4MKbazqOb
| +kEYdT5MiBEIJjQddhagbMWDYPF7McDSS/I3y4KdL1mI40Fjr6sXKOetrFvRZ+cC
| AwEAAaNDMEEwCQYDVR0TBAIwADA0BgNVHREELTArgilpcC0xMC0xMC0zMS04Mi5l
| dS13ZXN0LTEuY29tcHV0ZS5pbnRlcm5hbDANBgkqhkiG9w0BAQsFAAOCAQEArHg4
| zvCqUMzbSvusDU3d4cPDYnh7a7fAdOeVxHWo8/z/gzB8/ojJ8oYtfDV3qdKRhg0m
| pGSG3A2MZvl9u4FYj2tI8sne5HNTGRNg+3DLR/O9lFR90TH4v4piyAJrc29nFmpe
| Mq8I+JOizeSVG9qMSp6s0hDcHGAs111avS5TkEUvL0GybJIIQabOMDJ1e+Mptca+
| iV+Z+rdfirNzw87twkMxEpwTVPf3h5G0EKwE62Ih8cG1Pk/NrZCz5lN5P2b2BwHQ
| wbmbTgiA+hBmWajlHVu7EwEIsnMGrzTgSacVhHd7WsThLlMQwgNIowzUMagIA0yD
| s6SpR/+RIiQzeFiuTw==
|_-----END CERTIFICATE-----
|_smtp-commands: mail.filepath.lab, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, CHUNKING
|_ssl-date: TLS randomness does not represent time
110/tcp   open  pop3     syn-ack ttl 62 Dovecot pop3d
|_ssl-date: TLS randomness does not represent time
|_pop3-capabilities: PIPELINING SASL RESP-CODES CAPA TOP UIDL AUTH-RESP-CODE STLS
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Issuer: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2021-11-10T16:53:34
| Not valid after:  2031-11-08T16:53:34
| MD5:   05c8:4559:9811:a54f:9c53:b3ee:f6ad:f0fd
| SHA-1: a24d:7a7f:9ac1:8045:5c5f:5b7c:721a:4e21:0599:ed7c
| -----BEGIN CERTIFICATE-----
| MIIDOTCCAiGgAwIBAgIUZOpVp2fjesLBhoJHfQXOvrRFh2AwDQYJKoZIhvcNAQEL
| BQAwNDEyMDAGA1UEAwwpaXAtMTAtMTAtMzEtODIuZXUtd2VzdC0xLmNvbXB1dGUu
| aW50ZXJuYWwwHhcNMjExMTEwMTY1MzM0WhcNMzExMTA4MTY1MzM0WjA0MTIwMAYD
| VQQDDClpcC0xMC0xMC0zMS04Mi5ldS13ZXN0LTEuY29tcHV0ZS5pbnRlcm5hbDCC
| ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANCGzRe8Ucyrg1ICrmylNf/G
| Dhe8gGUJsnSdBBwzEhofznOXvjGJ/P+5/ScXSNm5Bb632xtPcZ3wSq9xHq8JqZMu
| oXjoyd4U6VK6aV4xjxlwdE33DgsAHXORv9PkMi+NeYDFsJrdRznSV64mc9xhIqSk
| WdnALkBXvNZcTwy6feITP3F4YTGa5ewRNJSVutU4hBY1CfroZxRnff6kkbF0iqQc
| dSHPjK3NeAZnp4iVID8rBuV/fjjOtZ53z1u6cXmQVc2fljvD4GN3TxV4MKbazqOb
| +kEYdT5MiBEIJjQddhagbMWDYPF7McDSS/I3y4KdL1mI40Fjr6sXKOetrFvRZ+cC
| AwEAAaNDMEEwCQYDVR0TBAIwADA0BgNVHREELTArgilpcC0xMC0xMC0zMS04Mi5l
| dS13ZXN0LTEuY29tcHV0ZS5pbnRlcm5hbDANBgkqhkiG9w0BAQsFAAOCAQEArHg4
| zvCqUMzbSvusDU3d4cPDYnh7a7fAdOeVxHWo8/z/gzB8/ojJ8oYtfDV3qdKRhg0m
| pGSG3A2MZvl9u4FYj2tI8sne5HNTGRNg+3DLR/O9lFR90TH4v4piyAJrc29nFmpe
| Mq8I+JOizeSVG9qMSp6s0hDcHGAs111avS5TkEUvL0GybJIIQabOMDJ1e+Mptca+
| iV+Z+rdfirNzw87twkMxEpwTVPf3h5G0EKwE62Ih8cG1Pk/NrZCz5lN5P2b2BwHQ
| wbmbTgiA+hBmWajlHVu7EwEIsnMGrzTgSacVhHd7WsThLlMQwgNIowzUMagIA0yD
| s6SpR/+RIiQzeFiuTw==
|_-----END CERTIFICATE-----
143/tcp   open  imap     syn-ack ttl 62 Dovecot imapd (Ubuntu)
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Issuer: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2021-11-10T16:53:34
| Not valid after:  2031-11-08T16:53:34
| MD5:   05c8:4559:9811:a54f:9c53:b3ee:f6ad:f0fd
| SHA-1: a24d:7a7f:9ac1:8045:5c5f:5b7c:721a:4e21:0599:ed7c
| -----BEGIN CERTIFICATE-----
| MIIDOTCCAiGgAwIBAgIUZOpVp2fjesLBhoJHfQXOvrRFh2AwDQYJKoZIhvcNAQEL
| BQAwNDEyMDAGA1UEAwwpaXAtMTAtMTAtMzEtODIuZXUtd2VzdC0xLmNvbXB1dGUu
| aW50ZXJuYWwwHhcNMjExMTEwMTY1MzM0WhcNMzExMTA4MTY1MzM0WjA0MTIwMAYD
| VQQDDClpcC0xMC0xMC0zMS04Mi5ldS13ZXN0LTEuY29tcHV0ZS5pbnRlcm5hbDCC
| ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANCGzRe8Ucyrg1ICrmylNf/G
| Dhe8gGUJsnSdBBwzEhofznOXvjGJ/P+5/ScXSNm5Bb632xtPcZ3wSq9xHq8JqZMu
| oXjoyd4U6VK6aV4xjxlwdE33DgsAHXORv9PkMi+NeYDFsJrdRznSV64mc9xhIqSk
| WdnALkBXvNZcTwy6feITP3F4YTGa5ewRNJSVutU4hBY1CfroZxRnff6kkbF0iqQc
| dSHPjK3NeAZnp4iVID8rBuV/fjjOtZ53z1u6cXmQVc2fljvD4GN3TxV4MKbazqOb
| +kEYdT5MiBEIJjQddhagbMWDYPF7McDSS/I3y4KdL1mI40Fjr6sXKOetrFvRZ+cC
| AwEAAaNDMEEwCQYDVR0TBAIwADA0BgNVHREELTArgilpcC0xMC0xMC0zMS04Mi5l
| dS13ZXN0LTEuY29tcHV0ZS5pbnRlcm5hbDANBgkqhkiG9w0BAQsFAAOCAQEArHg4
| zvCqUMzbSvusDU3d4cPDYnh7a7fAdOeVxHWo8/z/gzB8/ojJ8oYtfDV3qdKRhg0m
| pGSG3A2MZvl9u4FYj2tI8sne5HNTGRNg+3DLR/O9lFR90TH4v4piyAJrc29nFmpe
| Mq8I+JOizeSVG9qMSp6s0hDcHGAs111avS5TkEUvL0GybJIIQabOMDJ1e+Mptca+
| iV+Z+rdfirNzw87twkMxEpwTVPf3h5G0EKwE62Ih8cG1Pk/NrZCz5lN5P2b2BwHQ
| wbmbTgiA+hBmWajlHVu7EwEIsnMGrzTgSacVhHd7WsThLlMQwgNIowzUMagIA0yD
| s6SpR/+RIiQzeFiuTw==
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
|_imap-capabilities: more listed post-login have IMAP4rev1 LOGIN-REFERRALS ID STARTTLS Pre-login capabilities LOGINDISABLEDA0001 OK IDLE ENABLE SASL-IR LITERAL+
993/tcp   open  ssl/imap syn-ack ttl 62 Dovecot imapd (Ubuntu)
|_imap-capabilities: more listed post-login have IMAP4rev1 LOGIN-REFERRALS AUTH=PLAIN ENABLE AUTH=LOGINA0001 capabilities Pre-login OK IDLE ID SASL-IR LITERAL+
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Issuer: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2021-11-10T16:53:34
| Not valid after:  2031-11-08T16:53:34
| MD5:   05c8:4559:9811:a54f:9c53:b3ee:f6ad:f0fd
| SHA-1: a24d:7a7f:9ac1:8045:5c5f:5b7c:721a:4e21:0599:ed7c
| -----BEGIN CERTIFICATE-----
| MIIDOTCCAiGgAwIBAgIUZOpVp2fjesLBhoJHfQXOvrRFh2AwDQYJKoZIhvcNAQEL
| BQAwNDEyMDAGA1UEAwwpaXAtMTAtMTAtMzEtODIuZXUtd2VzdC0xLmNvbXB1dGUu
| aW50ZXJuYWwwHhcNMjExMTEwMTY1MzM0WhcNMzExMTA4MTY1MzM0WjA0MTIwMAYD
| VQQDDClpcC0xMC0xMC0zMS04Mi5ldS13ZXN0LTEuY29tcHV0ZS5pbnRlcm5hbDCC
| ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANCGzRe8Ucyrg1ICrmylNf/G
| Dhe8gGUJsnSdBBwzEhofznOXvjGJ/P+5/ScXSNm5Bb632xtPcZ3wSq9xHq8JqZMu
| oXjoyd4U6VK6aV4xjxlwdE33DgsAHXORv9PkMi+NeYDFsJrdRznSV64mc9xhIqSk
| WdnALkBXvNZcTwy6feITP3F4YTGa5ewRNJSVutU4hBY1CfroZxRnff6kkbF0iqQc
| dSHPjK3NeAZnp4iVID8rBuV/fjjOtZ53z1u6cXmQVc2fljvD4GN3TxV4MKbazqOb
| +kEYdT5MiBEIJjQddhagbMWDYPF7McDSS/I3y4KdL1mI40Fjr6sXKOetrFvRZ+cC
| AwEAAaNDMEEwCQYDVR0TBAIwADA0BgNVHREELTArgilpcC0xMC0xMC0zMS04Mi5l
| dS13ZXN0LTEuY29tcHV0ZS5pbnRlcm5hbDANBgkqhkiG9w0BAQsFAAOCAQEArHg4
| zvCqUMzbSvusDU3d4cPDYnh7a7fAdOeVxHWo8/z/gzB8/ojJ8oYtfDV3qdKRhg0m
| pGSG3A2MZvl9u4FYj2tI8sne5HNTGRNg+3DLR/O9lFR90TH4v4piyAJrc29nFmpe
| Mq8I+JOizeSVG9qMSp6s0hDcHGAs111avS5TkEUvL0GybJIIQabOMDJ1e+Mptca+
| iV+Z+rdfirNzw87twkMxEpwTVPf3h5G0EKwE62Ih8cG1Pk/NrZCz5lN5P2b2BwHQ
| wbmbTgiA+hBmWajlHVu7EwEIsnMGrzTgSacVhHd7WsThLlMQwgNIowzUMagIA0yD
| s6SpR/+RIiQzeFiuTw==
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
995/tcp   open  ssl/pop3 syn-ack ttl 62 Dovecot pop3d
|_ssl-date: TLS randomness does not represent time
|_pop3-capabilities: PIPELINING SASL(PLAIN LOGIN) USER CAPA TOP UIDL AUTH-RESP-CODE RESP-CODES
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Issuer: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2021-11-10T16:53:34
| Not valid after:  2031-11-08T16:53:34
| MD5:   05c8:4559:9811:a54f:9c53:b3ee:f6ad:f0fd
| SHA-1: a24d:7a7f:9ac1:8045:5c5f:5b7c:721a:4e21:0599:ed7c
| -----BEGIN CERTIFICATE-----
| MIIDOTCCAiGgAwIBAgIUZOpVp2fjesLBhoJHfQXOvrRFh2AwDQYJKoZIhvcNAQEL
| BQAwNDEyMDAGA1UEAwwpaXAtMTAtMTAtMzEtODIuZXUtd2VzdC0xLmNvbXB1dGUu
| aW50ZXJuYWwwHhcNMjExMTEwMTY1MzM0WhcNMzExMTA4MTY1MzM0WjA0MTIwMAYD
| VQQDDClpcC0xMC0xMC0zMS04Mi5ldS13ZXN0LTEuY29tcHV0ZS5pbnRlcm5hbDCC
| ASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBANCGzRe8Ucyrg1ICrmylNf/G
| Dhe8gGUJsnSdBBwzEhofznOXvjGJ/P+5/ScXSNm5Bb632xtPcZ3wSq9xHq8JqZMu
| oXjoyd4U6VK6aV4xjxlwdE33DgsAHXORv9PkMi+NeYDFsJrdRznSV64mc9xhIqSk
| WdnALkBXvNZcTwy6feITP3F4YTGa5ewRNJSVutU4hBY1CfroZxRnff6kkbF0iqQc
| dSHPjK3NeAZnp4iVID8rBuV/fjjOtZ53z1u6cXmQVc2fljvD4GN3TxV4MKbazqOb
| +kEYdT5MiBEIJjQddhagbMWDYPF7McDSS/I3y4KdL1mI40Fjr6sXKOetrFvRZ+cC
| AwEAAaNDMEEwCQYDVR0TBAIwADA0BgNVHREELTArgilpcC0xMC0xMC0zMS04Mi5l
| dS13ZXN0LTEuY29tcHV0ZS5pbnRlcm5hbDANBgkqhkiG9w0BAQsFAAOCAQEArHg4
| zvCqUMzbSvusDU3d4cPDYnh7a7fAdOeVxHWo8/z/gzB8/ojJ8oYtfDV3qdKRhg0m
| pGSG3A2MZvl9u4FYj2tI8sne5HNTGRNg+3DLR/O9lFR90TH4v4piyAJrc29nFmpe
| Mq8I+JOizeSVG9qMSp6s0hDcHGAs111avS5TkEUvL0GybJIIQabOMDJ1e+Mptca+
| iV+Z+rdfirNzw87twkMxEpwTVPf3h5G0EKwE62Ih8cG1Pk/NrZCz5lN5P2b2BwHQ
| wbmbTgiA+hBmWajlHVu7EwEIsnMGrzTgSacVhHd7WsThLlMQwgNIowzUMagIA0yD
| s6SpR/+RIiQzeFiuTw==
|_-----END CERTIFICATE-----
4000/tcp  open  http     syn-ack ttl 62 Node.js (Express middleware)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Sign In
50000/tcp open  http     syn-ack ttl 62 Apache httpd 2.4.41 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: System Monitoring Portal
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.41 (Ubuntu)
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.15
OS details: Linux 4.15
```

I know there are two open ports with HTTP service via `4000` and `50000`. Let's enumerate hidden directories

### Port 4000
```
ffuf -u http://10.48.188.141:4000/FUZZ -w /usr/share/wordlists/dirb/big.txt 2>/dev/null
Index                   [Status: 302, Size: 29, Words: 4, Lines: 1, Duration: 179ms]
fonts                   [Status: 301, Size: 177, Words: 7, Lines: 11, Duration: 178ms]
images                  [Status: 301, Size: 179, Words: 7, Lines: 11, Duration: 173ms]
index                   [Status: 302, Size: 29, Words: 4, Lines: 1, Duration: 180ms]
signin                  [Status: 200, Size: 1295, Words: 366, Lines: 41, Duration: 179ms]
signout                 [Status: 302, Size: 29, Words: 4, Lines: 1, Duration: 179ms]
signup                  [Status: 500, Size: 1246, Words: 55, Lines: 11, Duration: 179ms]
```

### Port 50000
```
ffuf -u http://10.48.188.141:50000/FUZZ -w /usr/share/wordlists/dirb/big.txt 2>/dev/null
.htaccess               [Status: 403, Size: 281, Words: 20, Lines: 10, Duration: 3481ms]
.htpasswd               [Status: 403, Size: 281, Words: 20, Lines: 10, Duration: 4499ms]
javascript              [Status: 301, Size: 328, Words: 20, Lines: 10, Duration: 175ms]
phpmyadmin              [Status: 403, Size: 281, Words: 20, Lines: 10, Duration: 177ms]
server-status           [Status: 403, Size: 281, Words: 20, Lines: 10, Duration: 179ms]
templates               [Status: 301, Size: 327, Words: 20, Lines: 10, Duration: 176ms]
uploads                 [Status: 301, Size: 325, Words: 20, Lines: 10, Duration: 177ms]
```

