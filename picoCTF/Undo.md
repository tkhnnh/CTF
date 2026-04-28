URL : [Undo](https://play.picoctf.org/practice/challenge/766?category=5&difficulty=1&page=1)


This service is terminal based which I need to input the right command to get the flag

```
===Welcome to the Text Transformations Challenge!===

Your goal: step by step, recover the original flag.
At each step, you'll see the transformed flag and a hint.
Enter the correct Linux command to reverse the last transformation.

--- Step 1 ---
Current flag: KTBxcDI0bnIwLWZhMDFnQHplMHNmYTRlRy1nazNnLXRhMWZlcmlyRShTR1BicHZj
Hint: Base64 encoded the string.
Enter the Linux command to reverse it: base64 -d
Correct!

--- Step 2 ---
Current flag: )0qp24nr0-fa01g@ze0sfa4eG-gk3g-ta1ferirE(SGPbpvc
Hint: Reversed the text.
Enter the Linux command to reverse it: rev
Correct!

--- Step 3 ---
Current flag: cvpbPGS(Eriref1at-g3kg-Ge4afs0ez@g10af-0rn42pq0)
Hint: Replaced underscores with dashes.
Enter the Linux command to reverse it: tr _ -
Incorrect. Try again.
Output: cvpbPGS(Eriref1at-g3kg-Ge4afs0ez@g10af-0rn42pq0)
Enter the Linux command to reverse it: tr - _
Correct!

--- Step 4 ---
Current flag: cvpbPGS(Eriref1at_g3kg_Ge4afs0ez@g10af_0rn42pq0)
Hint: Replaced curly braces with parentheses.
Enter the Linux command to reverse it: tr () {}
Correct!

--- Step 5 ---
Current flag: cvpbPGS{Eriref1at_g3kg_Ge4afs0ez@g10af_0rn42pq0}
Hint: Applied ROT13 to letters.
Enter the Linux command to reverse it: tr 'A-Za-z' 'N-ZA-Mn-za-m'
Correct!

Congratulations! You've recovered the original flag:
>>>
```

Happy Hacking!@$@#!$
