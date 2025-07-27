## This is the documentation of how I setting up my own Window Sever 2016 with Virtual Box
# Download Virtual Box
[Virtual Box Download](https://www.virtualbox.org/)
# Download Window Server ISO
Access to [Microsoft Evaluation Center](https://www.microsoft.com/en-us/evalcenter) site
<img width="1899" height="873" alt="Image" src="https://github.com/user-attachments/assets/487675d5-ef66-4735-9aff-11de52c0f011" />
Then in the navigation bar, click on `Windows Server` directory and hover `Windows Server` option
<img width="637" height="438" alt="Image" src="https://github.com/user-attachments/assets/cdd9b95d-f423-4f5d-8659-75631faec233" />
Choose the option `Download the ISO`
<img width="1893" height="872" alt="Image" src="https://github.com/user-attachments/assets/4046b3ef-75ab-4525-9aee-be543a4b8f97" />
=>Filling every empty space (Can be fabricated information)

Afterward, select the option `64-bit edition` depending on our own preference
<img width="1076" height="194" alt="Image" src="https://github.com/user-attachments/assets/4aff2467-ada7-41f0-83c9-5b93549b48dd" />
# Set up the server in Virtual Box
<img width="339" height="69" alt="Image" src="https://github.com/user-attachments/assets/29865e7f-52b3-4a83-b023-d2dcd7b18fe7" />
Select `New` option then a pop up window would appear
<img width="1255" height="686" alt="Image" src="https://github.com/user-attachments/assets/f1d1dec4-4a2c-4eb5-99a7-63bb20e92f5c" />
Enter the name of the VM then enter the `Finish` button, which after right click on the Window VM then open `Settings` by clicking on it or use short-cut `Ctrl-S`
<img width="616" height="578" alt="Image" src="https://github.com/user-attachments/assets/544c1ee6-dada-48d8-8d0b-f5de54d43086" />
After opening the setting, head toward `Storage` option in order to insert the iso file of the server
<img width="1282" height="594" alt="Screenshot 2025-07-27 145953" src="https://github.com/user-attachments/assets/b50ac222-57e3-4f3e-bafa-90a219a35494" />
After finishing those steps, start booting the machine by clicking `Start`
<img width="993" height="741" alt="Screenshot 2025-07-27 150148" src="https://github.com/user-attachments/assets/358b44df-91c0-45b8-9419-eba76262724d" />

Click on the `Next` button
<img width="999" height="746" alt="Screenshot 2025-07-27 150235" src="https://github.com/user-attachments/assets/ec0a47fd-cd31-43c9-820c-5468a3ed331c" />

Click on `Install`
<img width="997" height="742" alt="Screenshot 2025-07-27 150258" src="https://github.com/user-attachments/assets/a6db70b6-f2ba-4917-a97b-92fe2a7541c4" />

The first option only gives us a `Command Prompt`, what I want is to set up a GUI server so I choose second option
<img width="997" height="742" alt="Screenshot 2025-07-27 150421" src="https://github.com/user-attachments/assets/96883b90-10cd-491f-b213-85bd72afedb5" />

Click on accept license terms and `Next`
<img width="998" height="745" alt="Screenshot 2025-07-27 150617" src="https://github.com/user-attachments/assets/af14a02a-b725-4a06-9097-156ad5e4da1d" />

Choose the option `Custom: Install Windows only (advanced)` opiton
<img width="630" height="478" alt="Screenshot 2025-07-27 151138" src="https://github.com/user-attachments/assets/8f4cbd37-81a3-48e8-b9a5-14965af4a271" />
<img width="502" height="193" alt="Screenshot 2025-07-27 151146" src="https://github.com/user-attachments/assets/0b3ff364-7286-4d92-8a13-366cae35ed2d" />

I want to organize and dirrentiate system files so I choose the function `New` then apply whenever a warning window pop up just hit `Ok`, finally hit `Next`
Then waiting for Installing process, after successfully installing Window the machine will automatically restart.

# Customize settings
After customize my own password, I can see the look screen of the window server but any key inputs is not working so I navigate to the `Input` directory in the navigation bar

Choose `Keyboard` then `Insert Ctrl+Alt+Del` option and I am good to go.

# Setting up Domain Controller
