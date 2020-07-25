---
layout: writeup
category: zh3r0-CTF
chall_description: https://i.imgur.com/HdYCN73.png
points: 472
solves: 
tags: forensics stego crypto
date: 2020-07-25
comments: true
---

The given file is `image`

Running `xxd image | tail` gives:
<pre 
    class="command-line" 
    data-prompt="$" 
    data-output="2-11"
><code class="language-bash">xxd image | tail
00003480: ff30 2225 372a 3322 321d 1c13 241c 212b  .0"%7*3"2...$.!+
00003490: 1819 1e1a 2019 1416 1318 1412 140f 0f14  .... ...........
000034a0: 0f12 160e 1212 160f 1410 1517 130d 0b0a  ................
000034b0: 0c10 0e0b 0b0b 0a0b 090b 0b08 0a08 0a09  ................
000034c0: 0900 4300 dbff 3d6f 414e 5156 5862 3363  ..C...=oANQVXb3c
000034d0: 6b52 764e 4862 3130 6a64 2f67 3259 3046  kRvNHb10jd/g2Y0F
000034e0: 3264 7630 3262 6a35 535a 6956 4864 3139  2dv02bj5SZiVHd19
000034f0: 5765 7563 3364 3339 794c 364d 4863 3052  Weuc3d39yL6MHc0R
00003500: 4861 3e00 feff 0000 0100 0100 0001 0100  Ha>.............
00003510: 4649 464a 1000 e0ff d8ff                 FIFJ......</code>
</pre>

The last few bytes when reversed would give `ffd8 ffe0` which is the magic number of JPEG files. This suggests that the bytes of the file are reversed.
The bytes are then reversed using the following python script to get `image.jpeg`:
```python
file = open('image', 'rb')
bytearr = list(file.read())
open('image.jpeg', 'wb').write(bytes(reversed(bytearr)))
```
The content of image.jpeg is 

![image](https://i.imgur.com/Fhl3YPT.png)

Running `exiftool` on the image reveals a base64 encoded comment `aHR0cHM6Ly93d3cueW91dHViZS5jb20vd2F0Y2g/dj01bHNvRkc3bXVQNAo=`

![image](https://i.imgur.com/1TUMLqq.png)

On decoding the base64 encoded comment, a link to a [youtube video](https://www.youtube.com/watch?v=5lsoFG7muP4) titled “Kapela - Rock My Way”. This suggests the use of RockYou.txt.
Running `stegcracker` on the file reveals the password `spongebob`

![image](https://i.imgur.com/cAap0Wo.png)

The cracked file contains:
```plain-text
In [35]: n,e,ct,p+q
Out[35]: 
(156935655500198733255923805969370297538115753312746380213875723177744608509780722798549730106834861986575848272630355804840179947615966722051370804273521733290376009020885919941338141950993008276537987193794648055241515380150115338397065198086893695560540379329063476893211153270247222670504019722793971516489,
 65537,
102778142076243116117419062640171713879684005471846556860689446479305435562766590357152362175278713093609670819423506015563433111872029023117856369287465874159889936283732420732086482645886112577942492103417960605158427793203017078930148395937563028135853490687072326149444788825363901282252753328289332801180,
25089219254058723086004960979954103479984362695038160907003438818016936688465630366701002710571334149929206994096775851785636272938202242921638312612784566)`
```
This is classic RSA. 

$$
\begin{align}
\phi &= (p - 1) \cdot (q - 1) \\
    &= pq - (p + q) + 1 \\
    &= n - (p + q) + 1 \\
\end{align}
$$

Now we have `n`, `e` and `phi`. So, the cipher-text can be deciphered even without knowing the individual values of `p` and `q`.

Decoding the cipher-text gives the flag **`zh3r0{W0ah_Y0u_W0n_k33p_1t_uP}`**