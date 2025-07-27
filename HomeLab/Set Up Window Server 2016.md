## This is the documentation of how I setting up my own Window Sever 2016 with Virtual Box
# Download Virtual Box
[Virtual Box Download](https://www.virtualbox.org/)
# Download Window Server ISO
Access to [Microsoft Evaluation Center](https://www.microsoft.com/en-us/evalcenter) site.
<img width="1899" height="873" alt="Image" src="https://github.com/user-attachments/assets/487675d5-ef66-4735-9aff-11de52c0f011" />
Then in the navigation bar, click on `Windows Server` directory and hover `Windows Server` option.
<img width="637" height="438" alt="Image" src="https://github.com/user-attachments/assets/cdd9b95d-f423-4f5d-8659-75631faec233" />
Choose the option `Download the ISO`.
<img width="1893" height="872" alt="Image" src="https://github.com/user-attachments/assets/4046b3ef-75ab-4525-9aee-be543a4b8f97" />
=>Filling every empty space (Can be fabricated information).

Afterward, select the option `64-bit edition` depending on our own preference.
<img width="1076" height="194" alt="Image" src="https://github.com/user-attachments/assets/4aff2467-ada7-41f0-83c9-5b93549b48dd" />
# Set up the server in Virtual Box
<img width="339" height="69" alt="Image" src="https://github.com/user-attachments/assets/29865e7f-52b3-4a83-b023-d2dcd7b18fe7" />
Select `New` option then a pop up window would appear.
<img width="1255" height="686" alt="Image" src="https://github.com/user-attachments/assets/f1d1dec4-4a2c-4eb5-99a7-63bb20e92f5c" />
Enter the name of the VM then enter the `Finish` button, which after right click on the Window VM then open `Settings` by clicking on it or use short-cut `Ctrl-S`.
<img width="616" height="578" alt="Image" src="https://github.com/user-attachments/assets/544c1ee6-dada-48d8-8d0b-f5de54d43086" />
After opening the setting, head toward `Storage` option in order to insert the iso file of the server.
<img width="1282" height="594" alt="Screenshot 2025-07-27 145953" src="https://github.com/user-attachments/assets/b50ac222-57e3-4f3e-bafa-90a219a35494" />
After finishing those steps, start booting the machine by clicking `Start`.
<img width="993" height="741" alt="Screenshot 2025-07-27 150148" src="https://github.com/user-attachments/assets/358b44df-91c0-45b8-9419-eba76262724d" />

Click on the `Next` button.
<img width="999" height="746" alt="Screenshot 2025-07-27 150235" src="https://github.com/user-attachments/assets/ec0a47fd-cd31-43c9-820c-5468a3ed331c" />

Click on `Install`.
<img width="997" height="742" alt="Screenshot 2025-07-27 150258" src="https://github.com/user-attachments/assets/a6db70b6-f2ba-4917-a97b-92fe2a7541c4" />

The first option only gives us a `Command Prompt`, what I want is to set up a GUI server so I choose second option.
<img width="997" height="742" alt="Screenshot 2025-07-27 150421" src="https://github.com/user-attachments/assets/96883b90-10cd-491f-b213-85bd72afedb5" />

Click on accept license terms and `Next`.
<img width="998" height="745" alt="Screenshot 2025-07-27 150617" src="https://github.com/user-attachments/assets/af14a02a-b725-4a06-9097-156ad5e4da1d" />

Choose the option `Custom: Install Windows only (advanced)` opiton.
<img width="630" height="478" alt="Screenshot 2025-07-27 151138" src="https://github.com/user-attachments/assets/8f4cbd37-81a3-48e8-b9a5-14965af4a271" />
<img width="502" height="193" alt="Screenshot 2025-07-27 151146" src="https://github.com/user-attachments/assets/0b3ff364-7286-4d92-8a13-366cae35ed2d" />

I want to organize and dirrentiate system files so I choose the function `New` then apply whenever a warning window pop up just hit `Ok`, finally hit `Next`.
Then waiting for Installing process, after successfully installing Window the machine will automatically restart.

# Customize settings
After customize my own password, I can see the look screen of the window server but any key inputs is not working so I navigate to the `Input` directory in the navigation bar.

Choose `Keyboard` then `Insert Ctrl+Alt+Del` option and I am good to go.

# Setting up Domain Controller
Whenever I access to the desktop of Server machine, a `Server Manager` will pop up.
<img width="1026" height="860" alt="Screenshot 2025-07-27 154508" src="https://github.com/user-attachments/assets/a22d5c7e-689d-4f6f-bfeb-82e2f5168e11" />
Navigate to `Manage` option on the top right of the Dashboard, then select `Add Roles and Features`.
<img width="1026" height="860" alt="Screenshot 2025-07-27 154823" src="https://github.com/user-attachments/assets/20c4c947-8447-4c73-a93d-fe5f1d6b1af1" />

Select `Next` option in the next 3 images
<img width="1026" height="860" alt="Screenshot 2025-07-27 154853" src="https://github.com/user-attachments/assets/e17baa10-7263-4658-a0fa-830b263e8aad" />
<img width="1026" height="860" alt="Screenshot 2025-07-27 154859" src="https://github.com/user-attachments/assets/4f6819e9-82a3-45d1-92c7-f0a6698740ea" />
<img width="1026" height="860" alt="Screenshot 2025-07-27 154904" src="https://github.com/user-attachments/assets/d46f6a1b-1c87-4f8f-a304-415aa655f505" />
Remember to tick AD DS option in this phase
<img width="1026" height="860" alt="Screenshot 2025-07-27 154919" src="https://github.com/user-attachments/assets/0fcbdabd-1e6d-471d-8eb5-45b4d9d98010" />
Continuing hitting the `Next` button
<img width="1026" height="860" alt="Screenshot 2025-07-27 154927" src="https://github.com/user-attachments/assets/c2901b7f-7d80-46e2-911d-35993df1f740" />
<img width="1026" height="860" alt="Screenshot 2025-07-27 154934" src="https://github.com/user-attachments/assets/c450e940-2fd9-4910-a4b8-2b0b71eefd24" />
Then install it
<img width="1026" height="860" alt="Screenshot 2025-07-27 154940" src="https://github.com/user-attachments/assets/0da6f583-c90c-4f12-bc31-c4038789e06a" />
<img width="1026" height="860" alt="Screenshot 2025-07-27 154950" src="https://github.com/user-attachments/assets/c6b18c4f-a2ab-4829-a2fe-2a6c21f96e2d" />
Noticing the `Dashboard` I can easily see that there is a warning sign in the flag, then clicking on it
<img width="1026" height="860" alt="Screenshot 2025-07-27 155434" src="https://github.com/user-attachments/assets/ef00bbcb-bcba-4062-8c0f-45a4cc33b067" />

Select the option `Promote this server to a domain controller`. The `Active Directory Domain Services Configuration Wizard` window pop up, select `Add a new forest` option. I custom my root domain name `MARVEL.local` then hit `Next`
<img width="1026" height="860" alt="Screenshot 2025-07-27 155745" src="https://github.com/user-attachments/assets/e963aa3e-c076-4938-b0bb-e3f240a21a97" />
Custom my own password and hit `Next`.
Finalising everything then hit install. After the installing process comes to an end the machine will automatically restart.


