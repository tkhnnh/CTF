# Overview
<img width="1920" height="807" alt="Screenshot_2025-09-28_09-52-49" src="https://github.com/user-attachments/assets/984c656d-2a4f-45c5-82f2-aa1f09b553b6" />

# Actions
The trick is no matter how hard I tried to intercept the request with `BurpSuite`, I couldn't do that which means that I had to switch to manually brute-force. For some reason, I got the right `username` which is `admin` but I cannot guess the password
Having a look at the page source of the code, I found there were 2  accessible files such as index.js and css file.
<img width="1920" height="44" alt="Screenshot_2025-09-28_10-34-11" src="https://github.com/user-attachments/assets/a8932d76-0e05-4a7d-b30c-5f454bc4878c" />

I assume that the encoded string must be the flag so I use [cyberchef](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&input=Y0dsamIwTlVSbnMxTTNKMk0zSmZOVE55ZGpOeVh6VXpjbll6Y2w4MU0zSjJNM0pmTlROeWRqTnlmUQ) to decode 
<img width="1293" height="596" alt="Screenshot_2025-09-28_10-35-10" src="https://github.com/user-attachments/assets/ea8b9b9b-91ec-4e21-92d8-745403bd5e0e" />

Happy Hacking!!!!
