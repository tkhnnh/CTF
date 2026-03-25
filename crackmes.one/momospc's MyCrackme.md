Link: 
[momospc's MyCrackme](https://www.crackmes.one/crackme/6941aea10992a052ab22239d)


```
rw-rw-r--  1 ubuntu ubuntu  6185 Dec 17 06:10 mycrackme.zip
```


The password to unzip this file is `crackmes.one`
```
unzip mycrackme.zip
````

Using the command `strings` to find unobfuscated strings from the program and I found these
```
strings MyCrackM

super_secret_password
password: 
access granted
nope
```
