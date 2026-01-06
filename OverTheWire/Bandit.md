# Level 0 -> 1

```
bandit0@bandit:~$ cat readme
Congratulations on your first steps into the bandit game!!
Please make sure you have read the rules at https://overthewire.org/rules/
If you are following a course, workshop, walkthrough or other educational activity,
please inform the instructor about the rules as well and encourage them to
contribute to the OverTheWire community so we can keep these games free!

The password you are looking for is: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

# Level 1 -> 2
```
bandit1@bandit:~$ ll
total 24
-rw-r-----   1 bandit2 bandit1   33 Oct 14 09:26 -
drwxr-xr-x   2 root    root    4096 Oct 14 09:26 ./
drwxr-xr-x 150 root    root    4096 Oct 14 09:29 ../
-rw-r--r--   1 root    root     220 Mar 31  2024 .bash_logout
-rw-r--r--   1 root    root    3851 Oct 14 09:19 .bashrc
-rw-r--r--   1 root    root     807 Mar 31  2024 .profile
bandit1@bandit:~$ cat ./-
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

# Level 2 -> 3
```
bandit2@bandit:~$ cat ./--spaces\ in\ this\ filename--
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

# Level 3 -> 4

```
bandit3@bandit:~$ cat  inhere/...Hiding-From-You
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

# Level 4 -> 5
```
bandit4@bandit:~$ cat inhere/-file01
���0��w�8���q����Y���d����ZCF��+bandit4@bandit:~$ cat inhere/-file02
���     L����Q�.��`▒/▒��r
�P{bandit4@bandit:~$ cat inhere/-file03
���?�▒��1'�JV����,��2��
                       f�=����[?2004hbandit4@bandit:~$ cat inhere/-file04
��us���*��w��Z ��Ї|��@�Sq-bandit4@bandit:~$ cat inhere/-file05
W�cF���[Q
�
 ��a~���\0�ed(��ڨWbandit4@bandit:~$ cat inhere/-file06
?z=J"��oyvbandit4@bandit:~$ cat inhere/-file07
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

# Level 5 -> 6
```
bandit5@bandit:~/inhere$ find ./../ -type f -size 1033c 
./../inhere/maybehere07/.file2
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```
# Level 6 -> 7
```
bandit6@bandit:~$ find / -type f -size 33c -user bandit7 -group bandit6 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

# Level 7 -> 8
```
bandit7@bandit:~$ cat data.txt | grep "millionth"
millionth       dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

# Level 8 -> 9
```
bandit8@bandit:~$ sort  data.txt | uniq -u
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

```

# Level 9 -> 10
```
strings data.txt
--snip--
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

# Level 10 -> 11
```
bandit10@bandit:~$ cat data.txt | base64 -d
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

# Level 11 -> 12
```
bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

# Level 12 -> 13
first revert the hexdump
```
bandit12@bandit:/tmp/fake$ mv data.txt hexdump_data
bandit12@bandit:/tmp/fake$ xxd -r hexdump_data compressed_data
bandit12@bandit:/tmp/fake$ file compressed_data
compressed_data: gzip compressed data, was "data2.bin", last modified: Tue Oct 14 09:26:00 2025, max compression, from Unix, original size modulo 2^32 572

```
Tackling the repeatedly compressed file
```
bandit12@bandit:/tmp/fake$ mv compressed_data data.gz
bandit12@bandit:/tmp/fake$ gzip -d data.gz
bandit12@bandit:/tmp/fake$ file data
data: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/fake$ mv data data.bz2
bandit12@bandit:/tmp/fake$ bzip2 -d data.bz2
bandit12@bandit:/tmp/fake$ file data
data: gzip compressed data, was "data4.bin", last modified: Tue Oct 14 09:26:00 2025, max compression, from Unix, original size modulo 2^32 20480
bandit12@bandit:/tmp/fake$ mv data data.gz
bandit12@bandit:/tmp/fake$ gzip -d data.gz
bandit12@bandit:/tmp/fake$ file data
data: POSIX tar archive (GNU)
bandit12@bandit:/tmp/fake$ mv data data.tar
bandit12@bandit:/tmp/fake$ tar -xf data.tar
```
Now I got `data5.bin`
```
bandit12@bandit:/tmp/fake$  mv data5.bin data5.bin.tar
bandit12@bandit:/tmp/fake$ tar -xf data5.bin.tar
bandit12@bandit:/tmp/fake$ ll
total 476
drwxrwxr-x    2 bandit12 bandit12   4096 Dec 11 12:55 ./
drwxrwx-wt 1816 root     root     438272 Dec 11 12:56 ../
-rw-r--r--    1 bandit12 bandit12  10240 Oct 14 09:26 data5.bin.tar
-rw-r--r--    1 bandit12 bandit12    219 Oct 14 09:26 data6.bin
-rw-rw-r--    1 bandit12 bandit12  20480 Dec 11 12:42 data.tar
-rw-r-----    1 bandit12 bandit12   2581 Dec 11 12:33 hexdump_data
bandit12@bandit:/tmp/fake$ file data6.bin
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/fake$ mv data6.bin data6.bin.bz2
bandit12@bandit:/tmp/fake$ bzip2 -d data6.bin.bz2
bandit12@bandit:/tmp/fake$ ll
total 484
drwxrwxr-x    2 bandit12 bandit12   4096 Dec 11 12:56 ./
drwxrwx-wt 1816 root     root     438272 Dec 11 12:56 ../
-rw-r--r--    1 bandit12 bandit12  10240 Oct 14 09:26 data5.bin.tar
-rw-r--r--    1 bandit12 bandit12  10240 Oct 14 09:26 data6.bin
-rw-rw-r--    1 bandit12 bandit12  20480 Dec 11 12:42 data.tar
-rw-r-----    1 bandit12 bandit12   2581 Dec 11 12:33 hexdump_data
bandit12@bandit:/tmp/fake$ file data6.bin
data6.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/fake$ mv data6.bin data6.bin.tar
bandit12@bandit:/tmp/fake$ tar -xf data6.bin.tar
bandit12@bandit:/tmp/fake$ ll
total 488
drwxrwxr-x    2 bandit12 bandit12   4096 Dec 11 12:57 ./
drwxrwx-wt 1817 root     root     438272 Dec 11 12:57 ../
-rw-r--r--    1 bandit12 bandit12  10240 Oct 14 09:26 data5.bin.tar
-rw-r--r--    1 bandit12 bandit12  10240 Oct 14 09:26 data6.bin.tar
-rw-r--r--    1 bandit12 bandit12     79 Oct 14 09:26 data8.bin
-rw-rw-r--    1 bandit12 bandit12  20480 Dec 11 12:42 data.tar
-rw-r-----    1 bandit12 bandit12   2581 Dec 11 12:33 hexdump_data
bandit12@bandit:/tmp/fake$ file data8.bin
data8.bin: gzip compressed data, was "data9.bin", last modified: Tue Oct 14 09:26:00 2025, max compression, from Unix, original size modulo 2^32 49
bandit12@bandit:/tmp/fake$ mv data8.bin data.gz
bandit12@bandit:/tmp/fake$ gzip -d data.gz
bandit12@bandit:/tmp/fake$ ll
total 488
drwxrwxr-x    2 bandit12 bandit12   4096 Dec 11 12:59 ./
drwxrwx-wt 1818 root     root     438272 Dec 11 12:59 ../
-rw-r--r--    1 bandit12 bandit12     49 Oct 14 09:26 data
-rw-r--r--    1 bandit12 bandit12  10240 Oct 14 09:26 data5.bin.tar
-rw-r--r--    1 bandit12 bandit12  10240 Oct 14 09:26 data6.bin.tar
-rw-rw-r--    1 bandit12 bandit12  20480 Dec 11 12:42 data.tar
-rw-r-----    1 bandit12 bandit12   2581 Dec 11 12:33 hexdump_data
bandit12@bandit:/tmp/fake$ file data
data: ASCII text
bandit12@bandit:/tmp/fake$ cat data
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn

```
# Level 13 -> 14
```

bandit13@bandit:~$ ll
total 24
drwxr-xr-x   2 root     root     4096 Oct 14 09:26 ./
drwxr-xr-x 150 root     root     4096 Oct 14 09:29 ../
-rw-r--r--   1 root     root      220 Mar 31  2024 .bash_logout
-rw-r--r--   1 root     root     3851 Oct 14 09:19 .bashrc
-rw-r--r--   1 root     root      807 Mar 31  2024 .profile
-rw-r-----   1 bandit14 bandit13 1679 Oct 14 09:26 sshkey.private
bandit13@bandit:~$ exit
logout
Connection to bandit.labs.overthewire.org closed.


scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .
backend: gibson-0
bandit13@bandit.labs.overthewire.org's password: 
Permission denied, please try again.
bandit13@bandit.labs.overthewire.org's password: 
Permission denied, please try again.
bandit13@bandit.labs.overthewire.org's password: 
sshkey.private

```
After getting the key, I must change the permission in order to use the key to `700`
```
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```
```
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```


# Level 14 -> 15

Using `nc` command to connect to localhost on port 30000, use the password from the above section
```
bandit14@bandit:~$ nc localhost 30000
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

# Level 15 -> 16
```
bandit15@bandit:~$ echo "8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo" | openssl s_client -ign_eof -connect localhost:30001
CONNECTED(00000003)
<SNIP>
---
read R BLOCK
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

closed
```

# Level 16 -> 17
Check what hosts are available 
```
bandit16@bandit:~$ nmap localhost -p 31000-32000
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-01-04 20:19 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00011s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.06 seconds
```
Then just manally checking each port
```
bandit16@bandit:~$ echo "kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx" | openssl s_client -ign_eof  -connect localhost:31790
CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = SnakeOil
verify return:1
---
Certificate chain
 0 s:CN = SnakeOil
   i:CN = SnakeOil
   a:PKEY: rsaEncryption, 4096 (bit); sigalg: RSA-SHA256
   v:NotBefore: Jun 10 03:59:50 2024 GMT; NotAfter: Jun  8 03:59:50 2034 GMT
---
Server certificate

<SNIP>

---
read R BLOCK
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

closed

```
So this is a private key for the next level 
NOTE : give it READ permission(700) in order to use the key



# Level 17 -> 18

```
bandit17@bandit:~$ diff passwords.new passwords.old
42c42
< x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
---
> pGozC8kOHLkBMOaL0ICPvLV1IjQ5F1VA
```
The first one is the password since I got the byebye text

# Level 18 -> 19
```
ssh bandit18@bandit.labs.overthewire.org -p 2220 -T
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-1
bandit18@bandit.labs.overthewire.org's password: 
ls
readme
cat readme
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```
The flag `-T` ,which disables pseudo-terminal allocation. This often causes the remote shell to behave as a non-interactive shell, which might skip the bashrc depending on how it is written. 

# Level 19 -> 20
```
bandit19@bandit:~$ ll
total 36
drwxr-xr-x   2 root     root      4096 Oct 14 09:26 ./
drwxr-xr-x 150 root     root      4096 Oct 14 09:29 ../
-rwsr-x---   1 bandit20 bandit19 14884 Oct 14 09:26 bandit20-do*
-rw-r--r--   1 root     root       220 Mar 31  2024 .bash_logout
-rw-r--r--   1 root     root      3851 Oct 14 09:19 .bashrc
-rw-r--r--   1 root     root       807 Mar 31  2024 .profile
```
Given the `-s` sticky bit which mean any user can execute or modify that file like root user -> I am going to learn how it works
```
bandit19@bandit:~$ ./bandit20-do 
Run a command as another user.
  Example: ./bandit20-do whoami
```
It allows me to execute commands on behalf of `bandit20` user which is a form of privilege escalation

```
bandit19@bandit:~$ ./bandit20-do file /etc/bandit_pass/bandit20
/etc/bandit_pass/bandit20: ASCII text
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

# Level 20 -> 21
Setting up a listener which is our network daemon (whatever port nubmer) and also display the password line of bandit 20
```
bandit20@bandit:~$ nc -lnvp 1234 < /etc/bandit_pass/bandit20
Listening on 0.0.0.0 1234
Connection received on 127.0.0.1 47292
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
```
then using suconnect file to retrieve the password
```
bandit20@bandit:~$ ./suconnect 1234
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
```

# Level 21 -> 22
```
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```

# Level 22 -> 23
```
bandit22@bandit:~$ cat /etc/cron.d/cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:~$ cat usr/bin/cronjob_bandit23.sh
cat: usr/bin/cronjob_bandit23.sh: No such file or directory
bandit22@bandit:~$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```
Let's get the mytarget value
```
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
```

And get the password
```
bandit22@bandit:~$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
```

# Level 23 -> 24
```
bandit23@bandit:~$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

get the current user name which is bandit24 and assigned it to myname variable
Then move to ../foo directory and attempting to delete all the file */* however, any files which user owner bandit23, the script will stop and run the banid23's script instead.


Every had been set up in the `/tmp/rand` folder . So our goal is to make the script run automatically and copy the password into our file
```
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/rand/password
```
Now lets move this script to the interval cron job folder and wait for it execution
```
cp script.sh /var/spool/bandit24
```

Finally 
```
bandit23@bandit:/tmp/rand$ cat password
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

# Level 24 -> 25

```
bandit24@bandit:~$ mktemp -d
/tmp/tmp.3c1TFipjvp
bandit24@bandit:~$ cd /tmp/tmp.3c1TFipjvp
bandit24@bandit:/tmp/tmp.3c1TFipjvp$ nano script.sh
Unable to create directory /home/bandit24/.local/share/nano/: No such file or directory
It is required for saving/loading search history or cursor positions.

bandit24@bandit:/tmp/tmp.3c1TFipjvp$ ll
total 10020
drwx------    2 bandit24 bandit24     4096 Jan  4 22:15 ./
drwxrwx-wt 2324 root     root     10244096 Jan  4 22:15 ../
-rw-rw-r--    1 bandit24 bandit24      146 Jan  4 22:15 script.sh
bandit24@bandit:/tmp/tmp.3c1TFipjvp$ chmod +x script.sh 
bandit24@bandit:/tmp/tmp.3c1TFipjvp$ ./script.sh 
bandit24@bandit:/tmp/tmp.3c1TFipjvp$ ll
total 10512
drwx------    2 bandit24 bandit24     4096 Jan  4 22:16 ./
drwxrwx-wt 2324 root     root     10244096 Jan  4 22:16 ../
-rw-rw-r--    1 bandit24 bandit24   380000 Jan  4 22:16 poss.txt
-rw-rw-r--    1 bandit24 bandit24   121918 Jan  4 22:16 result.txt
-rwxrwxr-x    1 bandit24 bandit24      146 Jan  4 22:15 script.sh*
bandit24@bandit:/tmp/tmp.3c1TFipjvp$ sort result.txt | grep -v "Wrong!"

Correct!
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```

script.sh
```
#!/bin/bash

for i in {0000..9999}
do 
        echo gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i >> poss.txt
done

cat poss.txt | nc localhost 30002 > result.txt
```

# Level 25 -> 26

get the key first by connecting to bandit25 then usinng `scp`

```
scp -P 2220 bandit25@bandit.labs.overthewire.org:bandit26.sshkey .
```
now , we have the key to get in bandit 26
```
ssh bandit26@bandit.labs.overthewire.org -p 2220 -i bandit26.sshkey
```
However, I got instant timeout and got kicked out immediately whenever I attempted to connect to that
<img width="598" height="235" alt="Screenshot_2026-01-06_19-24-39" src="https://github.com/user-attachments/assets/88a86c17-0e7c-4e0d-b27f-a486d82b2cf3" />

Let's utilise `/etc/passwd/` to find more info about the bandit26 shell interpreter
```
bandit25@bandit:~$ cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
bandit25@bandit:~$ cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```
So I know that when never I connect to bandit26, the terminal will execute `more` command and display the ascii text, so my goal is to use the subshell inside the text editor to change the shell into `/bin/bash`.

In order to achive that I have to minimize the terminal window as small as possible to break the `more` command when displaying the ascii text
<img width="307" height="190" alt="Screenshot_2026-01-06_19-33-38" src="https://github.com/user-attachments/assets/6e4b2af5-dfde-4804-affa-2f45a207e458" />

Now, press V to enter vim mode
then
```
:set shell=/bin/bash
:shell #execute the shell
```
# Level 26 -> 27
```
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit\_pass/bandit27                                                                                                                                                                             
upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
```

# Level 27 -> 28



```
┌──(bmo㉿Bmo)-[~]
└─$ git clone  ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
Cloning into 'repo'...
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit27-git@bandit.labs.overthewire.org's password: 
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.

┌──(bmo㉿Bmo)-[~/repo]
└─$ cat README         
The password to the next level is: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN

```

# Level 28 -> 29
```
┌──(bmo㉿Bmo)-[~]
└─$ git clone  ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
Cloning into 'repo'...
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

backend: gibson-0
bandit28-git@bandit.labs.overthewire.org's password: 
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 9 (delta 2), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (9/9), done.
Resolving deltas: 100% (2/2), done

┌──(bmo㉿Bmo)-[~/repo]
└─$ ll
total 4
-rw-rw-r-- 1 bmo bmo 111 Jan  6 19:56 README.md
                                                                                                                    
┌──(bmo㉿Bmo)-[~/repo]
└─$ cat README.md
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx

```
The password has been obfuscated so check the log to see something fun
```
┌──(bmo㉿Bmo)-[~/repo]
└─$ git log                                                                              
commit b0354c7be30f500854c5fc971c57e9cbe632fef6 (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Tue Oct 14 09:26:19 2025 +0000

    fix info leak

commit d0cf2ab7dd7ebc6075b59102a980155268f0fe8f
Author: Morla Porla <morla@overthewire.org>
Date:   Tue Oct 14 09:26:19 2025 +0000

    add missing data

commit bd6bc3a57f81518bb2ce63f5816607a754ba730d
Author: Ben Dover <noone@overthewire.org>
Date:   Tue Oct 14 09:26:18 2025 +0000

    initial commit of README.md
                                                                                                                    
┌──(bmo㉿Bmo)-[~/repo]
└─$ git show b0354c7be30f500854c5fc971c57e9cbe632fef6
commit b0354c7be30f500854c5fc971c57e9cbe632fef6 (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Tue Oct 14 09:26:19 2025 +0000

    fix info leak

diff --git a/README.md b/README.md
index d4e3b74..5c6457b 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials
 
 - username: bandit29
-- password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
+- password: xxxxxxxxxx
```
I attempted to view the commit fix into leak and hurray I found the password for bandit 29 

# Level 29 -> 30
```
┌──(bmo㉿Bmo)-[~/repo]
└─$ git checkout dev                                 
branch 'dev' set up to track 'origin/dev'.
Switched to a new branch 'dev'
                                                                                                                    
┌──(bmo㉿Bmo)-[~/repo]
└─$ cat README.md   
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL

```
# Level 30 -> 31


# Level 31 -> 32

# Level 32 ->
