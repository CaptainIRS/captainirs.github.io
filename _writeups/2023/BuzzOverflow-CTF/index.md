---
layout: ctf_overview
title: BuzzOverflow CTF
category: BuzzOverflow-CTF
date: 2023-11-06
---

# BuzzOverflow CTF Writeups

## Reversing

### xxxxx

Find and replace the function calls with the function return values and then cleanup the code. Then after some modifications we can get the following code to find the flag:

```py
a = [3,3,1, 3,1,3, 3, 1, 1,3, 1,1,3, 1, 3, 3,1, 1, 1, 13,1, 1, 1,  1, 1,  3, 1, 1]
b = [4,8,67,1,7,1, 13,11,7,11,4,5,17,19,17,4,41,19,11,2, 17,19,1,  3, 109,17,53,25]
c = [4,1,1, 4,2,41,1, 2, 1,1, 4,4,1, 1, 2, 4,2, 1, 1, 4, 1, 1, 1,  1, 1,  1, 1, 1]
d = [1,5,1, 7,5,1, 3, 5, 7,3, 3,5,1, 5, 1, 1,1, 5, 5, 1, 3, 5, 109,17,1,  1, 1, 5] 

flag = [''] * 28
flag[0] = chr(a[0] * b[0] * c[0] * d[0])
flag[1] = chr(a[1] * b[1] * c[1] * d[1])
flag[1+1] = chr(a[1+1] * b[1+1] * c[1+1] * d[1+1])
flag[1+1+1] = chr(a[1+1+1] * b[1+1+1] * c[1+1+1] * d[1+1+1])
flag[1+1+1+1] = chr(a[1+1+1+1] * b[1+1+1+1] * c[1+1+1+1] * d[1+1+1+1])
flag[1+1+1+1+1] = chr(a[1+1+1+1+1] * b[1+1+1+1+1] * c[1+1+1+1+1] * d[1+1+1+1+1])
flag[1+1+1+1+1+1] = chr(a[1+1+1+1+1+1] * b[1+1+1+1+1+1] * c[1+1+1+1+1+1] * d[1+1+1+1+1+1])
flag[1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1])
flag[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] = chr(a[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * b[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * c[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1] * d[1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1+1])
print(''.join(flag))
```

Flag: `0xCTF{un1c0d3_f0R_7h3_m3m35}`

### elfish

It looks like the magic number of the ELF binary is corrupted.

![](https://i.imgur.com/xnXkfJo.png)

Modify the first four bytes of the file using a tool like [Hexed.it](https://hexed.it) to the elf headers (7F, 'E', 'L', 'F').

Change the permissions and run the modified binary to get the flag:

Flag: `0xCTF{y_U_g0tta_b_s0_elf1$h}`

## Forensics

### Pixelated secrets 1

Opening the image with stegsolve, there seems to be some hidden data in the blue plane:

![](https://imgur.com/RBz4nbE.png)

So trying to extract the information from the blue plane using python:

```py
from PIL import Image
im = Image.open('easy_version.png')
px = im.load()

def decode_binary_string(s):
    return ''.join(chr(int(s[i*8:i*8+8],2)) for i in range(len(s)//8))

print(decode_binary_string(''.join([str(px[0, y][2]) for y in range(256)])))
```

Flag: `CTF{22s05a2005}`

### Uber Bug Bounty

There is a GMX file given in the chall. I'm not sure what the file actually is, but running strings on the file reveals this:

![](https://imgur.com/qraH7OS.png)

Here we can see there is an embedded Thumbnail file (JPEG). So using a hexedit program, deleting the bytes from the beginning till `<Thumbnail.jpg>` and from `</Thumbnail.jpg>` till the end, we get the jpeg image:

![](https://imgur.com/apGNJ6j.png)

On rotating and flipping the image, we get the flag:

Flag: `CTF{FASTASF}`

### Pixelated Secrets 2

This is similar to Pixelated Secrets 1, but the data is in the first row instead of the first column.

Extracting the data:

```py
from PIL import Image
im = Image.open('hard_version.png')
px = im.load()

def decode_binary_string(s):
    return ''.join(chr(int(s[i*8:i*8+8],2)) for i in range(len(s)//8))

res = ''.join([str(px[x, 0][2]) for x in range(256)])
print(res)
```

We get the binary string: `0001000000000101000100010001000000010000000101000001010101000101000100010000010100000101000001010001010000000101000101010001000100010101000001000001010000010001000100010101010100000101000001000000010100000100000101010101000100000000000000000000000000000000`. There seems to be unnecessary zeros in every odd position. Getting rid of those:

```py
res = ''.join(res[x] for x in range(1, len(res), 2))
print(decode_binary_string(res))
```

Flag: `CTF{S3cure_22}`

## Crypto

### The Emperor's Secret

Ciphertext: Jodg_brx_uhphpehu_ph

Caesar cipher with shift 23: https://cryptii.com/pipes/caesar-cipher

Flag: `0xCTF{Glad_you_remember_me}`

### Nature's Secret

Chall:

![](https://imgur.com/M4iZ9DV.png)

Decode using bird cipher: https://www.dcode.fr/birds-on-a-wire-cipher

Flag: `0xCTF{birdsonawiregochirpchirpchip}`

### Does size matter?

Given n, d, ciphertext, we can get the plaintext:

```py
c = 46163498626677786993275746470510
n = 50537019332326445555049589372337
d = 49623239819855508877100187645953
print(pow(c,d,n))
```

The original message is: 531095210810895110, which is a concatenation of ascii characters. Decoding this:

```py
print(''.join(chr(x) for x in [53, 109, 52, 108, 108, 95, 110]))
```

Flag: `0xCTF{5m4ll_n}`

## OSINT

### Google's Your Friend

Find image using google reverse image search, find the height of the focus central equity tower.

Flag: `0xCTF{166}`

## Web

### Back To Time

Checking out https://clickme.herocharge.repl.co/ in DevTools, there is a link to https://clickme.herocharge.repl.co/flag.html. Visiting this in web archive, http://web.archive.org/web/20220616185603/https://clickme.herocharge.repl.co/flag.html we get the flag.

Flag: `CTF{waaaaaybaaaaackmaaaaachine}`
