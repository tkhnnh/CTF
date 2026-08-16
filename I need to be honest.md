Link: [pipedown's I need to be honest](https://crackmes.one/crackme/69cd43df49fa49a2a2602312)

Difficulty: 1.7

Arch: x84-64

Platform: Unix/linux etc.

Language: Assembler

# Solution
After downloading a file, checking the program with `file` utility
```
$ file ineedtobehonest 
ineedtobehonest: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, not stripped
```

So the binary file is `not stripped` which means all labels are being reserved for debugging 

For easy approach, use `strings` utility to read all the strings and labels in the binary 
```
$ strings ineedtobehonest 
================================================
   REVERSE ENGINEERING CHALLENGE v3.14159
================================================
<SNIP>
[!] Flags remain encrypted.
<FLAG></FLAG>
SecurePass_2k26_X64_Reverse
<FLAG>CRYPTO_KEY_ALPHA_2026</FLAG>{
uwuq{h
y<FLAG>REVERSE_ENGINEERING_CHALLENGE</FLAG>f
d<FLAG>MEMORY_HIDDEN_GAMMA_X64</FLAG>P* -+R!)!#>53$%(()"3+-!!-34ZXPC* -+R
ineedtobehonest.asm
<SNIP>
```

Alternatively, using decomplier platform such as `ghidra`. Inspecting `.data` section to retrieve the password in plaintext then retrieve three flags
<img width="624" height="528" alt="Screenshot 2026-08-16 at 12 37 33 pm" src="https://github.com/user-attachments/assets/9f65a0d7-6eac-449c-b702-5a431f3aefca" />


Password: SecurePass_2k26_X64_Reverse
flag1: CRYPTO_KEY_ALPHA_2026
flag2: REVERSE_ENGINEERING_CHALLENGE
flag3: MEMORY_HIDDEN_GAMMA_X64

Done Happy Cracking @!#@!#!@#!
