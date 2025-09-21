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

so the extension should be `png` and magic byte should match. However, to make it simple just add the word `PNG` at the top of the file
Using this site called [revshell.com](https://www.revshells.com/) to generate a php web shell

Then using `nano` to craft the payload. Imporatnt~~~!!! The extension should be doubld `*.png.php` since I already tested with `*.png` and `*.php.png` already

<img width="1913" height="931" alt="Screenshot_2025-09-21_16-10-21" src="https://github.com/user-attachments/assets/dcbd0e6e-2e63-4245-804f-5fc160d7a0c9" />
<img width="613" height="308" alt="Screenshot_2025-09-21_16-12-30" src="https://github.com/user-attachments/assets/23c47da6-1fd4-43e5-8f5b-e7e5d31d4045" />

Then I just need to upload the payload. Accessing the file to trigger the payload in the `/uploads/*.png.php`

<img width="1133" height="375" alt="Screenshot_2025-09-21_16-16-44" src="https://github.com/user-attachments/assets/2fb8b2ab-4fcc-4141-8fdd-99c1ce03ef2a" />
And using the follow command to find the flag since the flag extension is followed by `.txt`
```
fint / -name *.txt
```
the using `cat` to read the flag and that's it.
```
cat /var/www/html/HFQWKODGMIYTO.txt
```

Happy Hacking
