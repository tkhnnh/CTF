# Overview
The target is a webb app allowing to upload image
<img width="1170" height="178" alt="Screenshot_2025-09-21_14-07-43" src="https://github.com/user-attachments/assets/aafa4a1e-3dac-4b02-bb74-19279c622fc8" />

# Finding hidden directory

For whom want to know how can I come up with this endpoint `robots.txt` and `/uploads`, I use `ffuf` a tool helped me fuzz those endpoints to find out using this command
```
ffuf -c  -u "http://{link_of_target}/FUZZ" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```
That is how I fuzz endpoints. Inside `robots.txt`, I enumerated another file called `instructions.txt`
<img width="675" height="206" alt="Screenshot_2025-09-21_14-09-01" src="https://github.com/user-attachments/assets/4d158138-ca2a-415a-adcf-2757dd3ed5b4" />
Having a look at the   `instructions.txt`   file
<img width="1186" height="216" alt="Screenshot_2025-09-21_14-11-54" src="https://github.com/user-attachments/assets/39e32a21-e217-4edc-b6be-6c0ff8a9262c" />
so the extension should be `png` and magic byte should match
