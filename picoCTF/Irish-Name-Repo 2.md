For this room, even I tried sqlmap to check but returned no results. Things get so complicated, since I tried to encode, obfuscate the payload and they were not use.

Til I try with only `'` and the page responded
<img width="1920" height="960" alt="Screenshot_2025-11-14_00-37-12" src="https://github.com/user-attachments/assets/4565591b-e388-4110-a9fc-77e3c6221162" />

so I kept attempting and had the final payload
```
admin' --
```
I don't know why the character a in admin is not capitalized which make the payload `Admin' --` did not work
Anyway I got the flag.

Happy Hacking!!!!~~~
