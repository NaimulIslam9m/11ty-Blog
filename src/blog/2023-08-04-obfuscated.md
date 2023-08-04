---
title: Obfuscated
description: Easy obfuscated VBA code challenge in cyberdefenders
author: Naimul Islam
date: 2023-08-01T07:43:57.799Z
tags:
  - post
image: /assets/blog/obfuscation-1.png
imageAlt: Obfuscation
---
## 1. What is the sha256 hash of the doc file?
```
ff2c8cadaa0fd8da6138cce6fce37e001f53a5d9ceccd67945b15ae273f4d751
```

## 2. Multiple streams contain macros in this document. Provide the number of lowest one.
```shell
root@cee331db43d2 /shared# oledump.py sample    
 1:       114 '\x01CompObj'  
 2:       284 '\x05DocumentSummaryInformation'  
 3:       392 '\x05SummaryInformation'  
 4:      8017 '1Table'  
 5:      4096 'Data'  
 6:       483 'Macros/PROJECT'  
 7:        65 'Macros/PROJECTwm'  
 8: M    7117 'Macros/VBA/Module1'  
 9: m    1104 'Macros/VBA/ThisDocument'  
10:      3467 'Macros/VBA/_VBA_PROJECT'  
11:      2964 'Macros/VBA/__SRP_0'  
12:       195 'Macros/VBA/__SRP_1'  
13:      2717 'Macros/VBA/__SRP_2'  
14:       290 'Macros/VBA/__SRP_3'  
15:       565 'Macros/VBA/dir'  
16:        76 'ObjectPool/_1541577328/\x01CompObj'  
17: O   20301 'ObjectPool/_1541577328/\x01Ole10Native'  
18:      5000 'ObjectPool/_1541577328/\x03EPRINT'  
19:         6 'ObjectPool/_1541577328/\x03ObjInfo'  
20:    133755 'WordDocument'
```

## 3. What is the decryption key of the obfuscated code?
The doc file is a dropper. It drops `maintools.js` file. After executing the file we have to get it. I run it on anyrun and downloaded maintools.js binary. In anyrun process execution, we see maintools.js is executed with `EzZETcSXyKAdF_e5I2i1` argument. Which is the decryption key of this code.

## 4. What is the name of the dropped file?
maintools.js

## 5. This script uses what language?
jscript
## 6. What is the name of the variable that is assigned the command-line arguments?
```js
try {
    var wvy1 = WScript.Arguments;
    var ssWZ = wvy1(0);
    var ES3c = y3zb();
[trucated]
```

## 7. How many command-line arguments does this script expect?
1
## 8. What instruction is executed if this script encounters an error?
WScript.Quit()
## 9. What function returns the next stage of code (i.e. the first round of obfuscated code)?
y3zb
## 10. The function LXv5 is an important function, what variable is assigned a key string value in determining what this function does?
LUK7
## 11. What encoding scheme is this function responsible for decoding?
Base64
## 12. In the function CpPT, the first two for loops are responsible for what important part of this function?
key-scheduling algorithm
## 13. The function CpPT requires two arguments, where does the value of the first argument come from?
command-line argument
## 14. For the function CpPT, what does the first argument represent?
key
## 15. What encryption algorithm does the function CpPT implement in this script?
RC4
## 16. What function is responsible for executing the deobfuscated code?
eval
## 17. What Windows Script Host program can be used to execute this script in command-line mode?
cscript.exe, an alternative to wscript.exe
## 18. What is the name of the first function defined in the deobfuscated code?
UspD