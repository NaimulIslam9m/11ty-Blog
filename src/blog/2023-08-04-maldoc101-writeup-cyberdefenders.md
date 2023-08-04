---
title: MalDoc101 Writeup - CyberDefenders
description: Analyzing malicious MS office document file in cyberdefenders.
author: Naimul Islam
date: 2023-08-02T07:46:51.309Z
tags:
  - post
  - featured
image: http://www.gispp.org/wp-content/uploads/2022/03/Banner.png
imageAlt: maldoc
---
## 1.  Multiple streams contain macros in this document. Provide the number of highest one.
```
oledump.py sample.bin
```

![](https://i.imgur.com/zsuzYIi.png)

## 2. What event is used to begin the execution of the macros?
```
olevba sample.bin
```

![](https://i.imgur.com/1R92dS4.png)

## 3. What malware family was this maldoc attempting to drop?
Drop the sample into Virustotal. It shows `trojan.w97m/emotet`.

## 4. What stream is responsible for the storage of the base64-encoded string?
```bash
for i in {17..36}  
do  
   echo "stream $i"  
   oledump.py sample.bin -s $i  
done
```

Using the above script, I printed all the streams and found 34 number stream has base64 string.

## 5. This document contains a user-form. Provide the name?
roubhaol
## 6. This document contains an obfuscated base64 encoded string; what value is used to pad (or obfuscate) this string?
Dumped the stream in a file.

```
oledump.py sample.bin -s 34 | cut -d ' ' -f 20 > dump.txt
```

Then found `2342772g3&*gs7712ffvs626fq` string as repetitive.

## 7. What is the program executed by the base64 encoded string?
Removing the pad we get base64 string. Using cyberchef we get decoded Powershell command.

> **Recipe**: From Base64 > Remove null bytes
## 8. What WMI class is used to create the process to launch the trojan?
![](https://imgur.com/YlBNwJO.png)
## 9. Multiple domains were contacted to download a trojan. Provide first FQDN as per the provided hint.
After executing the URL part of powershell script we get the some urls. First one is the answer.

![](https://imgur.com/wynbFtH.png)


