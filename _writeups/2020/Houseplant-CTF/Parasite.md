---
layout: writeup
category: Houseplant-CTF
chall_description: https://i.imgur.com/viwWW1I.png
points: 784
solves: 
tags: crypto
date: 2020-07-25
comments: true
---

The given file Parasite.txt has the content: 

**`.---  -..  ..-    --  .  -.-     -.-  -..  ..-.     .--.  ..-  ..-.     .--.  -  -.-    .---  .  ..-.    .-..  ..-    --.  .  ..-  -.-    -.-.  ....  -.-    -.-  ..-  .--   ..-.  ..-    -...  .`**

The challenge hint says – “*Make sure you keep track of the spacing- it's there for a reason*”. Replacing all 3 space clusters with ‘/’ and decrypting the Morse Code, we get **`JDU MEK KDF PUF PTK JEF LU GEUK CHK KUW FU BE`**

**`.---  -..  ..-/ --  .  -.-/  -.-  -..  ..-./  .--.  ..-  ..-./  .--.  -  -.-/ .---  .  ..-./ .-..  ..-/ --.  .  ..-  -.-/ -.-.  ....  -.-/ -.-  ..-  .--/..-.  ..-/ -...  .`**

 ![CyberChef Morse Code](https://i.imgur.com/6QtQIas.png)

From, the random capitalization in the Challenge Description, it is quite evident that “SKATS” is hidden. Googling “SKATS” leads to a [Wikipedia Page](https://en.wikipedia.org/wiki/SKATS) which gives a table for translating the text to Korean:

![SKATS Table](https://i.imgur.com/AuRXxSR.png)
  
After substitution we get  **희망은진정한기생충입니다**

On translating it with Google Translate, we get:

![Google Translate Results](https://i.imgur.com/HTeyB2C.png)
 
Modifying it to match the flag format, we get the flag: **`rtcp{Hope_is_a_true_parasite}`**
