# Overview 
This is a bank website which allow user to enter the number and let the software do the calculation
<img width="619" height="158" alt="Screenshot_2025-09-20_19-17-37" src="https://github.com/user-attachments/assets/f03721df-4423-4a73-9f6f-c0361beef260" />

# Eval

what is `eval()`? According to my knowledge, `eval()` is a function that allow you to convert tpying input to command

# So what is the code that created this software ?
At first, I assumed that it would probably python due to this evidence below
<img width="669" height="243" alt="Screenshot_2025-09-20_19-16-51" src="https://github.com/user-attachments/assets/008b600c-19b7-4298-b00b-8119a85dd230" />

After having a look at the page source, I can assure that it is python
<img width="866" height="115" alt="Screenshot_2025-09-20_19-15-52" src="https://github.com/user-attachments/assets/1f45c9f1-b642-4bc2-bbc4-8e19af4acf6c" />

However, there are some keywords which are restricted in this site like os, ls, cat, etc. So there would be two step for getting this flag

# 1. Encode the command to bypass the filter
```
echo "cat /flag.txt" | base64
Y2F0IC9mbGFnLnR4dAo=
```
By coverting normal command into encoded string, then later decoding by abusing `eval()` function

# 2. Crafting the payload and execute
Because the software restrict the `import` keyword which means I have to use like an extended version of that code
```
__import__("subprocess").getoutput(__import__("base64").b64decode('Y2F0IC9mbGFnLnR4dAo='))
```
Inject this command and boom!! I got the flag.
