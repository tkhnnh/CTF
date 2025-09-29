# Overview
The target is a login form, I assume that the target is vulnerable to SQLi

# Actions
## Round 1
The hint : `You are not allowed to login with valid credentials.`
<img width="1919" height="621" alt="Screenshot_2025-09-29_22-37-19" src="https://github.com/user-attachments/assets/2100b131-79b0-41c3-8970-a489f8176a33" />

Also I have access to the filter as well
<img width="638" height="182" alt="Screenshot_2025-09-29_22-37-11" src="https://github.com/user-attachments/assets/a057b41d-4d20-41f3-a0b0-af98d5c20c98" />


I just need to enter one of the input, specifically the username field with this payload 
```
admin';--
```
and for the password just type random thing, because the query will ignore it anyway

## Round 2
<img width="1920" height="643" alt="Screenshot_2025-09-29_22-39-04" src="https://github.com/user-attachments/assets/7bbedd6a-40bb-4673-a6a1-296b21e41012" />
<img width="1831" height="252" alt="Screenshot_2025-09-29_22-39-21" src="https://github.com/user-attachments/assets/f53e3402-0f06-409f-a7bb-e0da6ac35e97" />

No more `--`, no worry, I got it
```
admin' /*
```
Moving to next round

## Round 3
<img width="1920" height="563" alt="Screenshot_2025-09-29_22-41-12" src="https://github.com/user-attachments/assets/b0bb7f85-da85-443b-9a51-2e3baa382c64" />
<img width="1920" height="215" alt="Screenshot_2025-09-29_22-41-24" src="https://github.com/user-attachments/assets/67b20408-f362-45ad-a383-f9e459c6fb6e" />

So I try the same payload of round 2, but it did not work out. What if I forgot `--`
```
admin';`
```
That is the answer. How unexpectedly!!

## Round 4
<img width="1920" height="630" alt="Screenshot_2025-09-29_22-42-19" src="https://github.com/user-attachments/assets/5be250a2-9ad8-414d-8a70-f5f08a5fec27" />

<img width="1920" height="206" alt="Screenshot_2025-09-29_22-44-24" src="https://github.com/user-attachments/assets/b32218ea-984b-46d9-93a2-35a955274045" />

OMG, now it filters out the string `admin` as well. So I try this
```
ad'||'min';
```
and it worked

## Round 5
<img width="1920" height="609" alt="Screenshot_2025-09-29_22-47-02" src="https://github.com/user-attachments/assets/7ecd6124-e80e-43fa-96f3-879e1b4db335" />
<img width="1920" height="155" alt="Screenshot_2025-09-29_22-47-21" src="https://github.com/user-attachments/assets/ed518101-9a82-425a-a5a4-9406ea03eae4" />
Still, the filter does not update anything new to me, so I just reuse the payload from round 4

## Time to get the flag

<img width="1920" height="568" alt="Screenshot_2025-09-29_22-49-02" src="https://github.com/user-attachments/assets/c555fe29-d127-46e3-bc29-cd83f6adad44" />
Reload the `filter.php`
<img width="1237" height="743" alt="Screenshot_2025-09-29_22-50-44" src="https://github.com/user-attachments/assets/a7f1538d-7812-44ee-ad2d-0113b4ece5df" />

Happy Hacking!!!!
