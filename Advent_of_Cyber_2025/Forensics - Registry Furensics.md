# Window Registry
Functioning like a brain in Window OS
Made up several separate files each storing information on different configuration settings -> `Hives`.

<img width="1233" height="935" alt="Screenshot_2025-12-17_13-43-05" src="https://github.com/user-attachments/assets/d157e401-6b4f-4270-92a9-b43f4b61703f" />


The Windows OS has a built-in tool known as the `Registry Editor`
<img width="1106" height="384" alt="68d2c1e7ab94268f6271de1d-1763584779702" src="https://github.com/user-attachments/assets/7a2b4289-2505-4c4a-a5ba-f22093fd1f72" />

Here is how we can spot on our `Hives`
<img width="1229" height="454" alt="Screenshot_2025-12-17_13-47-25" src="https://github.com/user-attachments/assets/e5570a07-951a-46ef-85bb-e2a553f7e6f0" />

# View USB devices connected
<img width="2056" height="1068" alt="68d2c1e7ab94268f6271de1d-1761292172298" src="https://github.com/user-attachments/assets/7b644bd4-896a-471b-b029-fbadca331516" />


# View Programs run by the User
<img width="2050" height="1076" alt="68d2c1e7ab94268f6271de1d-1761292172317" src="https://github.com/user-attachments/assets/ef45fed8-c519-44b3-abdf-a3d254014c43" />

# Registry Forensic
-> the process of extracting and analyzing evidence from the registry
<img width="1107" height="870" alt="Screenshot_2025-12-17_13-53-04" src="https://github.com/user-attachments/assets/233c3e92-5f3c-486b-832e-49e5a381bec4" />

the Registry Editor does not allow opening offline hives. The Register editor also displays some of the key values in binary which are not readable.
-> [Registry Explorer](https://ericzimmerman.github.io/#!index.md)


What application was installed on the dispatch-srv01 before the abnormal activity started?
The goal is to find the software -> need to investigate Software Hives
Path : `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall`
`DroneManager Updater`
<img width="639" height="50" alt="Screenshot_2025-12-17_14-20-02" src="https://github.com/user-attachments/assets/787cb39a-cb06-469d-a0b7-567fb89cad78" />

What is the full path where the user launched the application (found in question 1) from?
In term of launched application ->  NTUSER.DAT. Key: This key stores information on recently accessed applications launched via the GUI.
Path : ` ROOT\Software\Microsoft\Windows NT\CurrentVersion\AppCompatFlags\Compatibility Assistant\Store`
`C:\Users\dispatch.admin\Downloads\DroneManager_Setup.exe`
<img width="915" height="680" alt="Screenshot_2025-12-17_14-35-28" src="https://github.com/user-attachments/assets/03430523-f852-499f-98bf-e5e91ffa10cb" />


Which value was added by the application to maintain persistence on startup?
Application -> software 
Path `ROOT\Microsoft\Windows\CurrentVersion\Run`

`"C:\Program Files\DroneManager\dronehelper.exe" --background`
<img width="917" height="674" alt="Screenshot_2025-12-17_14-40-32" src="https://github.com/user-attachments/assets/e57d8b29-0eae-47da-866a-f6d562bd2560" />

