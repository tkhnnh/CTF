Unzipping the file with the password 
`crackmes.one`

# 1. Giving the file executable permissions by using the following command
```
chmod +x <filename>
```

# 2. Interacting with the software
```
$ ./simp-password      
Enter password: %41s
Wrong try again.
```
In this case, it probably need a valid password to get in so I am going to find readable string on this file by using the following commands
```
strings <filename>
```

as the output I found something interesting
```
<SNIP>
Enter password: 
%41s
iloveicecream
I love ice cream too!
Wrong try again.
<SNIP>
```

so where are those strings comming from ? is one of these the password? And yes, it is
```
$ ./simp-password
Enter password: iloveicecream
I love ice cream too!
```

# I did it. However, I want to see the internal of this software so here is the alternative approach using `gdb`

## The assembly code (intel format)
```
   0x00000000000011c9 <+0>:     endbr64
   0x00000000000011cd <+4>:     push   rbp
   0x00000000000011ce <+5>:     mov    rbp,rsp
   0x00000000000011d1 <+8>:     sub    rsp,0x40
   0x00000000000011d5 <+12>:    mov    rax,QWORD PTR fs:0x28
   0x00000000000011de <+21>:    mov    QWORD PTR [rbp-0x8],rax
   0x00000000000011e2 <+25>:    xor    eax,eax
   0x00000000000011e4 <+27>:    lea    rax,[rip+0xe19]        # 0x2004
   0x00000000000011eb <+34>:    mov    rdi,rax
   0x00000000000011ee <+37>:    mov    eax,0x0
   0x00000000000011f3 <+42>:    call   0x10b0 <printf@plt>
   0x00000000000011f8 <+47>:    lea    rax,[rbp-0x40]
   0x00000000000011fc <+51>:    mov    rsi,rax
   0x00000000000011ff <+54>:    lea    rax,[rip+0xe0f]        # 0x2015
   0x0000000000001206 <+61>:    mov    rdi,rax
   0x0000000000001209 <+64>:    mov    eax,0x0
   0x000000000000120e <+69>:    call   0x10d0 <__isoc99_scanf@plt>
   0x0000000000001213 <+74>:    lea    rax,[rbp-0x40]
   0x0000000000001217 <+78>:    lea    rdx,[rip+0xdfc]        # 0x201a
   0x000000000000121e <+85>:    mov    rsi,rdx
   0x0000000000001221 <+88>:    mov    rdi,rax
   0x0000000000001224 <+91>:    call   0x10c0 <strcmp@plt>
   0x0000000000001229 <+96>:    test   eax,eax
   0x000000000000122b <+98>:    jne    0x123e <main+117>
   0x000000000000122d <+100>:   lea    rax,[rip+0xdf4]        # 0x2028
   0x0000000000001234 <+107>:   mov    rdi,rax
   0x0000000000001237 <+110>:   call   0x1090 <puts@plt>
   0x000000000000123c <+115>:   jmp    0x124d <main+132>
   0x000000000000123e <+117>:   lea    rax,[rip+0xdf9]        # 0x203e
   0x0000000000001245 <+124>:   mov    rdi,rax
   0x0000000000001248 <+127>:   call   0x1090 <puts@plt>
   0x000000000000124d <+132>:   mov    eax,0x0
   0x0000000000001252 <+137>:   mov    rdx,QWORD PTR [rbp-0x8]
   0x0000000000001256 <+141>:   sub    rdx,QWORD PTR fs:0x28
   0x000000000000125f <+150>:   je     0x1266 <main+157>
   0x0000000000001261 <+152>:   call   0x10a0 <__stack_chk_fail@plt>
   0x0000000000001266 <+157>:   leave
   0x0000000000001267 <+158>:   ret
```

## Explainnation
As I am still a beginner, I am going to use offset as a breakpoint for simplicity
### main+42
so this one is going to print a string, assumably, the string `enter password: `

### main+69
as I can read the function name, it is scanf@plt which is a function inside procedure linked table responsible for taking input

### main+91
at this instruction, the program compares 2 string, I would assume that it compare the input of `main+69` with the defined password, then if they are equal -> move to `main+110` which is a put function, otherwise moving to  `main+132`

### main+110
put the string on the screen which probably the congratulation string

### main+132
print out the wrong password string

so narrow down the scope, I could find the string from 69->91 before comparing process
By manipulating the `rip` which is register instruction pointer

Starting from `rip+0xdfc`

```
(gdb) x/s $rip+0xdf6
0x55555555601a: "iloveicecream"
```

Happy Hacking~~~~
