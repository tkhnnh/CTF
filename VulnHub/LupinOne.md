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
nmap -T4 -sV -vv -O -A 192.168.0.155
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
nmap --script=vuln 192.168.0.155
```

<img width="691" height="308" alt="Screenshot 2025-09-18 235444" src="https://github.com/user-attachments/assets/a12c9c7d-3ef8-4c4c-8668-410c56a3cfa3" />

Further dive into `manual` folder to find more interesting files and here are some potential folders

<img width="787" height="246" alt="Screenshot 2025-09-19 000038" src="https://github.com/user-attachments/assets/1f68605c-139d-4c3a-b7d0-069a12b56071" />

`/da`,`/de`, `/en`, and the remain directories inside `/manual` folder will generate the same results. This is a sample list but all of them generated the same things.

<img width="795" height="197" alt="Screenshot 2025-09-19 000308" src="https://github.com/user-attachments/assets/48163bcf-017b-4f48-beab-6f18455810af" />

