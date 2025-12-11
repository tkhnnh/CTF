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

