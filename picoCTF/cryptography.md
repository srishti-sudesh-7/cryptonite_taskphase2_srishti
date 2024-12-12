# C3

_Flag:_ picoCTF{adlibs}

Procedure:
- ciphertext: ![image](https://github.com/user-attachments/assets/00fc5982-88fc-46f8-99e6-5621c1f6ab51)
- first I went through the given code, which was in Python, a language I'm not familiar with.
- I modified the code so that it no longer requires any imports.
  ![image](https://github.com/user-attachments/assets/eca46bab-ed62-4631-836e-812bf5cf869a)
- I tried to reverse this code, because my logic was that if I reversed it's operation and gave the ciphertext as the input, then I would get the original text as output:
  ![image](https://github.com/user-attachments/assets/bfc089ac-0a5f-46cb-b4d9-0340241138bc)
- when I tried to run this output code on an online compiler it showed an error as the print statement was missing the (). I had to look up why this was the case.
- Through chatgpt I found that I had to use python2, and that this could be done via my terminal.
- so I stored the new decoding file in a python file called new.py:                 
  ![image](https://github.com/user-attachments/assets/8958a0dd-ccd8-496f-af20-b5eda96b5767)

- then I used my terminal to find the answer:
  ![image](https://github.com/user-attachments/assets/09cb63f0-6c21-4355-8da1-be8a6608b195)                   
  here I piped `cat new.py` into the same program because its code mentions a comment `selfinput`, ie it takes itself as the input

Incorrect method:
- should have made use of my terminal more, instead I used an online compiler

Learnings:
- got more familiar with Python
- learnt how to think logically to reverse agiven encoding code to make it a decoding one

References:
ChatGPT
