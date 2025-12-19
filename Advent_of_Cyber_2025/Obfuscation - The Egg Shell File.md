Obfuscation is the practice of making data hard to read and analyze

# Detecting pattern

- ROT1 - common words look “one letter off”, spaces stay the same. Easy enough to detect.
- ROT13 - Look for three-letter words. Common ones like  the become gur. And and becomes naq. spaces stay the same.
- Base64 - Long strings containing mostly alphanumeric characters (i.e., A-Z, a-z, 0–9), sometimes with + or /, often ending in = or ==.
- XOR - A bit more tricky. Looks like random symbols but stays the same length as the original. If a short secret was reused, you may notice a tiny repeat every few characters.

# Unfamiliar Patterns
use `Magic` to guesses but with limit performance

<img width="341" height="142" alt="Screenshot_2025-12-19_14-51-17" src="https://github.com/user-attachments/assets/bea289ac-70a6-4185-ae8e-c2a3e9e79182" />

<img width="1528" height="870" alt="Screenshot_2025-12-19_14-50-42" src="https://github.com/user-attachments/assets/f3bd9dd9-3ffe-4522-b734-417c94acc1e0" />

<img width="1261" height="694" alt="Screenshot 2025-12-19 151814" src="https://github.com/user-attachments/assets/ca4b95c1-0efc-40b8-a1b2-62c9014573a3" />
<img width="480" height="207" alt="Screenshot 2025-12-19 151801" src="https://github.com/user-attachments/assets/8540bd31-b26b-4b2a-ad6d-cef794a047a4" />
