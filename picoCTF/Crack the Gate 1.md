# Approach 
The bypass way will be hidden some where like page source or hidden paths.


## Look up page source 
<img width="681" height="109" alt="Screenshot_2025-12-27_21-22-03" src="https://github.com/user-attachments/assets/805652fe-f4a6-4113-a516-d3f106541b36" />
Luckily I found it in the page source.


## Decode 
<img width="1246" height="543" alt="Screenshot_2025-12-27_21-21-53" src="https://github.com/user-attachments/assets/fc29a05b-306d-4390-acd7-fcfb00c681e5" />
I use cyberchef to decihper this encoding string which is a rotation of 13.

## Exploit
Finally, the way to get in is adding a header called `X-Dev-Access: Yes` which is a very bad habit putting the Dev access publicly to everyone who can access the site even though it is decoded 
<img width="1224" height="467" alt="Screenshot_2025-12-27_21-27-42" src="https://github.com/user-attachments/assets/89b930ef-46c1-45d5-8d4b-107096f9fb36" /><img width="499" height="255" alt="Screenshot_2025-12-27_21-27-22" src="https://github.com/user-attachments/assets/a2dc742c-13e3-4ca3-b2a5-0f3409fa530a" />



Happy Hacking ~!~~!
