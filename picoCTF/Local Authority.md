<img width="1920" height="933" alt="Screenshot_2025-09-07_22-59-11" src="https://github.com/user-attachments/assets/3b5e7ce8-8c31-466b-8cc6-f8688fdbc8da" />
I tried the combination admin:password and had a look at the page source in which I couldn't find anything that is useful
<img width="1917" height="929" alt="Screenshot_2025-09-07_22-58-54" src="https://github.com/user-attachments/assets/de15782d-e652-444f-aa22-5261e8095a23" />
I found the hash and tried to crack it but the hash was useless either
<img width="630" height="166" alt="Screenshot_2025-09-07_23-02-01" src="https://github.com/user-attachments/assets/1ac370dc-6d2d-47e0-906f-5b86893c2b4b" />
Then using inspect in Network tab I found the `secure.js` file and accessing it which leaded me to the credential that I need 
<img width="1920" height="932" alt="Screenshot_2025-09-07_22-59-20" src="https://github.com/user-attachments/assets/c01d26df-2e55-4e74-a206-55cca9a746a1" />
<img width="1920" height="452" alt="Screenshot_2025-09-07_23-11-58" src="https://github.com/user-attachments/assets/40ab4ec0-ec1f-43cb-8fbb-cd75365a0df0" />
<img width="1920" height="325" alt="Screenshot_2025-09-07_23-11-46" src="https://github.com/user-attachments/assets/19fa18b4-da32-4bb1-a1f8-b1888186e0a9" />
