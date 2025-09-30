# Overview
This is the target
<img width="1920" height="804" alt="Screenshot_2025-09-30_22-58-22" src="https://github.com/user-attachments/assets/0de1f72e-e3a2-460e-90ad-0f0ac1a5df4d" />

It is kinda the same with the room `picobrowser`. However, this time I am goint to use BurpSuite for convenience

<img width="1254" height="679" alt="Screenshot_2025-09-30_22-54-51" src="https://github.com/user-attachments/assets/ffedc05b-1e8d-4fff-a0e3-4cfac822050e" />
I just need to modify the `User-Agent` a little bit and I saw `don't trust visiting from another site`. So I put `Referer` header, which is acting like referer for new employee
<img width="1258" height="668" alt="Screenshot_2025-09-30_22-54-28" src="https://github.com/user-attachments/assets/61fd8129-1918-444f-8db9-6d4ed5b16ea0" />

NOw, it is about date and time, so I use `Date` header
<img width="1260" height="680" alt="Screenshot_2025-09-30_22-56-30" src="https://github.com/user-attachments/assets/1a86e783-1a91-4173-9b2f-36e2ef616c0d" />

It wants `do not track`, I search it up for header related to do not track and found `DNT` which is surprisingly stood for Do Not Track
<img width="1257" height="678" alt="Screenshot_2025-09-30_23-05-04" src="https://github.com/user-attachments/assets/08937e1a-aee4-4463-88a3-9922e8a85f78" />

OMG, Sweden, just look for some random IP `102.177.146.0` and use `X-forwared-for` 
<img width="1256" height="678" alt="Screenshot_2025-09-30_23-07-11" src="https://github.com/user-attachments/assets/8304395e-1f4c-4cc3-a8a5-49b031a11228" />

I already expected this would ask for language so I use `Accept-Language`. Finally
<img width="1258" height="680" alt="Screenshot_2025-09-30_23-08-40" src="https://github.com/user-attachments/assets/9b2c3280-1f80-4e0b-b9bf-d6daeea11363" />

Happy hacking!!!
