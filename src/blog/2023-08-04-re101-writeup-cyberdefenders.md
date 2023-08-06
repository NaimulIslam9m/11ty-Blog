---
title: RE101 Writeup - CyberDefenders
description: Easy reversing challenge of cyberdefenders
author: Naimul Islam
date: 2023-07-30T07:17:43.312Z
tags:
  - post
image: https://cyberdefenders.org/media/terraform/RE101/RE101.jpg
imageAlt: encrypted-code
---
## I've used this new encryption I heard about online for my warez; I bet you can't extract the flag! (File: MALWARE000)

die > strings
ZmxhZzwwb3BzX2lfdXNlZF8xMzM3X2I2NF9lbmNyeXB0aW9uPgo=
base64 -d

**Flag**: 0ops_i_used_1337_b64_encryption

## Check out what I can do! (File: Just some JS)
![](https://i.imgur.com/IX7Cayz.png)

Obfuscated javascript.
node just_some_js

**Flag**: what_a_cheeky_language!1!

## I'm tired of Javascript. Luckily, I found the grand-daddy of that lame last language! (File: This is not JS)
![](https://i.imgur.com/5UGxKzM.png)
BrainFuck!
https://www.dcode.fr/brainfuck-language

**Flag**: Now_THIS_is_programming 

## I zipped flag.txt and encrypted it with the password "password", but I think the header got messed up... You can have the flag if you fix the file (File: Unzip Me)

Used this [PKZip specification doc](https://users.cs.jmu.edu/buchhofp/forensics/formats/pkzip.html).
Just needed to fix file name length.

![](https://imgur.com/P9M5evA.png)
**Flag**: R3ad_th3_spec

## Apparently, my encryption isn't so secure. I've got a new way of hiding my flags! (File: MALWARE101)

THIS CHALLENGE IS REALLY TRICKY!
The scrambled characters injected into stack memory in order. 
So, by using GDB, adding breakpoint before printf, you will get the flag in stack.

![](https://imgur.com/rqqnrKK.png)
**Flag**: sTaCk_strings_LMAO

## Ugh... I guess I'll just roll my own encryption. I'm not too good at math, but it looks good to me! (File: MALWARE201)

```
x = [0x6d, 0x78, 0x61, 0x6c, 0xdd, 0x7e, 0x65, 0x7e, 0x47, 0x6a, 0x4f, 0xcc, 0xf7, 0xca, 0x73, 0x68, 0x55, 0x42, 0x53, 0xdc, 0xd7, 0xd4, 0x6b, 0xec, 0xdb, 0xd2, 0xe1, 0x1c, 0x6d, 0xde, 0xd1, 0xc2]

for i, num in enumerate(x):
    a = i | 0xa0
    b = num ^ a
    c = b >> 1
    print(chr(c), end='')
```

**Flag**: malwar3-3ncryp710n-15-Sh17