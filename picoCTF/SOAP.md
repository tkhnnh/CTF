# Concept
The room gave me a hint related to XML external entity injection. What is that?
A vulnerability that allows attacker to interfere with the XML data leads to data exposure

# Overview
Back to the target, I can see that it is a page illustrating news or announcements.
<img width="1920" height="933" alt="Screenshot_2025-09-22_10-22-15" src="https://github.com/user-attachments/assets/b2a9df56-11d7-4a2d-80a8-023755865ae3" />

 So What should I do now? Learn how to attack using XXE injection. How? go straight to [PortSwigger](https://portswigger.net/web-security/xxe) and I understand it now. In order to perform the attack, I must follow these steps:

1. Introduce (or edit) a `DOCTYPE` element that defines an external entity containing the path to the file.
   
2. Edit a data value in the XML that is returned in the application's response, to make use of the defined external entity.

Well, I intecept the request using `BurpSuite`. IMPORTANT: Intercept the one which contains the method POST only

<img width="613" height="531" alt="Screenshot_2025-09-22_10-26-50" src="https://github.com/user-attachments/assets/9aff40bb-fba6-4bec-8aa0-8a9fd13cbeeb" />

Then follow the instruction, I could not get any information at all, the `BurpSuite` kept disconnected the intercepting process based on the payload I got from `PortSwigger`.
I tried to look for more payloads and I ended up with this [list](https://github.com/payloadbox/xxe-injection-payload-list/blob/master/Intruder/xxe-injection-payload-list.txt.txt)
<img width="546" height="182" alt="Screenshot_2025-09-22_11-01-19" src="https://github.com/user-attachments/assets/b6307ab6-b4c6-4d97-ac4a-009215505c17" />

I looked for XXE: File Disclosure and got the right payload
<img width="1226" height="655" alt="Screenshot_2025-09-22_11-01-39" src="https://github.com/user-attachments/assets/0665b6ba-0172-4ad6-a3df-69757df39059" />
Happy hacking!!! Done
