![Image](https://github.com/user-attachments/assets/a303d68f-8ab5-4cf3-84d7-7f3264f723c4)
# I downloaded the `Task` from the THM site and the extension of the file was `.zip`.
## Using `unzip` to get files
![Image](https://github.com/user-attachments/assets/92ffa8c5-17cc-4ab9-b044-e00ee76b7aa8)
# I tried to use `cat` to view the content of `Tetrix.exe` but it accidentially ran nonstop which freaked me out
![Image](https://github.com/user-attachments/assets/887e1500-9d9d-4ac9-b2ed-4e24272e325b)

# Therefore, I had to use `file` to check the type of this suspicious thingy and found this
![Image](https://github.com/user-attachments/assets/5c4e99ba-1657-48c4-a28f-d3b8cffb247e)
## To be honest, I had no idea what is this file type but I noticed that during the running process of this code when I tried to use `cat` to view there actually included some printatble texts.
## Thus, I used `strings` and also filtered the head of the flag 
```python
stings Tetrix.exe | grep "THM"
```

# DONE!!!
