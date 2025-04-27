# Challenge Information
```text
Level: Easy 
Tags: picoCTF 2021, General Skills
Author: LT 'SYREAL' JONES
 
Description:
This file has a flag in plain sight (aka "in-the-clear"). Download flag.
 
Hints:
1. Any hints about entering a command into the Terminal (such as the next one), will start with a '$'... everything after the dollar sign will be typed (or copy and pasted) into your Terminal.
2.To get the file accessible in your shell, enter the following in the Terminal prompt: $ wget https://mercury.picoctf.net/static/a5683698ac318b47bd060cb786859f23/flag
3.$ man cat
```
# Solution
> [!NOTE]
> I'm using webshell, an existing tool on the website

Let copy the `link address` first.
Here is the link [Download flag](https://mercury.picoctf.net/static/a5683698ac318b47bd060cb786859f23/flag).

<img width="809" alt="Image" src="https://github.com/user-attachments/assets/5139c33e-8428-4813-8493-04063c49434f" />

Then I open the Webshell on the website and notice that I can use the second hint to get the flag.

