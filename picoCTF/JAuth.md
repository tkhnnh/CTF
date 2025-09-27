# Overview
With provided credential, the website does not provide any useful information at all
<img width="1772" height="417" alt="Screenshot_2025-09-27_23-24-34" src="https://github.com/user-attachments/assets/be4bd533-b1c5-493a-96a1-6d18da3da417" />
<img width="1889" height="178" alt="Screenshot_2025-09-27_23-24-56" src="https://github.com/user-attachments/assets/862118ff-28eb-441b-b750-004038c47be9" />

# Actions
First Using `BurpSuite` to intecept and get the token
<img width="1225" height="436" alt="Screenshot_2025-09-27_23-23-05" src="https://github.com/user-attachments/assets/d0341fcc-258c-44dd-8564-107eda361edc" />

Then using [jwt.io](https://www.jwt.io/) to modify the token to escalate privilege
<img width="1321" height="459" alt="Screenshot_2025-09-27_23-22-40" src="https://github.com/user-attachments/assets/bb9052fe-6daa-4d76-85e8-6878a8f3ad10" />

Using that token to get access as admin, but remember to switch the variable `isAdmin` to 1 as well
<img width="1220" height="651" alt="Screenshot_2025-09-27_23-22-25" src="https://github.com/user-attachments/assets/5d46cbd5-7c4e-447b-a08d-e79421563bf1" />

Happy Hacking!!!
