---
layout: writeup
category: Redpwn-CTF
chall_description: https://i.imgur.com/PDqAcIB.png
points: 128
solves: 1136
tags: n00b reversing
date: 2020-07-25
comments: true
---

This demonstrates the first step one must do when given a crackme - Run `strings`. This might give an insight about what to look for in a given [binary](/assets/CTFs/Redpwn-CTF/ropes).

So, running `strings`, the output is:

![strings output](https://i.imgur.com/MQwCnPB.png)

There, you can see the flag in two parts: `flag{r0pes_ar3_just_l0ng_str1ngs}`