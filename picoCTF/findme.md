# Overview
There are two redirecting at the first glance
<img width="1736" height="394" alt="Screenshot_2025-09-22_22-43-20" src="https://github.com/user-attachments/assets/aa426a55-c666-4d4a-bfa8-950fadb72af4" />

<img width="1437" height="279" alt="Screenshot_2025-09-22_22-43-09" src="https://github.com/user-attachments/assets/a41517cb-a174-4023-9307-5ce026a005c2" />

So Have a look at the source code I found the the function of suggesting in search bar has been disabled and there is no way I can guess the flag right in some where directories.
<img width="569" height="275" alt="Screenshot_2025-09-22_22-48-23" src="https://github.com/user-attachments/assets/610b9dcb-6d75-4cc0-a706-9648f4fc0a7f" />


# Actions
I use `BurpSuite` to tamper the requests and ended up with 2 additional redirecting pages
<img width="1254" height="756" alt="Screenshot_2025-09-22_22-43-51" src="https://github.com/user-attachments/assets/4fa0b928-348c-45fa-bbce-ca818940ca4e" />
<img width="1258" height="759" alt="Screenshot_2025-09-22_22-44-07" src="https://github.com/user-attachments/assets/64a6b6ba-e052-4afe-9a1f-98c5c5e87db7" />
<img width="1258" height="701" alt="Screenshot_2025-09-22_22-44-20" src="https://github.com/user-attachments/assets/6e44919c-b794-4d5b-97bd-392e882b55be" />
<img width="1258" height="700" alt="Screenshot_2025-09-22_22-44-32" src="https://github.com/user-attachments/assets/f86e1a1b-1c3f-48eb-aeb9-ee2373f532ca" />

Well, the id of each redirecting page is quite thrilling, so I attempt to decode it using `Decoder` in `BurpSuite` and got the flag when combining two ids together. I actually can use `cyberchef` for decoding section like this but I prefer `Decoder` over it due to convenience.
<img width="527" height="211" alt="Screenshot_2025-09-22_22-46-57" src="https://github.com/user-attachments/assets/69f42b1c-304e-430b-8ea1-92784a2b735a" />
Happy Haking!!!!
