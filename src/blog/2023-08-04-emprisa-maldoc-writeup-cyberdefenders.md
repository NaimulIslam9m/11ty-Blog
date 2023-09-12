---
title: Emprisa Maldoc Writeup - CyberDefenders
description: As a SOC analyst, you were asked to inspect a suspected document
  (RTF) a user received in his inbox. One of your colleagues told you that he
  could not find anything suspicious. However, throwing the document into the
  sandboxing solution triggered some alerts. Your job is to investigate the
  document further and confirm whether it's malicious or not.
author: Naimul Islam
date: 2023-08-04T15:28:15.050Z
tags:
  - post
image: https://cyberdefenders.org/media/terraform/Emprisa%20Maldoc/Emprisa_Maldoc.jpg
imageAlt: ""
---
> As a SOC analyst, you were asked to inspect a suspected document a user received in his inbox. One of your colleagues told you that he could not find anything suspicious. However, throwing the document into the sandboxing solution triggered some alerts.
>
> Your job is to investigate the document further and confirm whether it's malicious or not.

## 1. What is the CVE ID of the exploited vulnerability?
First extracted md5 hash and then search it in virustotal. There it is tagged with two CVE's. Earliest one is the answer.

MD5: d82341600606afcf027646ea42f285ae
CVE: CVE-2017-11882, CVE-2018-0802

## 2. To reproduce the exploit in a lab environment and mimic a corporate machine running Microsoft office 2007, a specific patch should not be installed. Provide the patch number.
After doing google search, found about the patch [in this page](https://support.microsoft.com/en-us/topic/description-of-the-security-update-for-2007-microsoft-office-suite-november-28-2017-7f275c8d-14df-8be5-3f9c-b3761bf9681d). 
The patch ID is KB4011276.

## 3. What is the magic signature in the object data?
After running rtfdump.py tool, we get the magic number.
magic: d0cf11e0


## 4. What is the name of the spawned process when the document gets opened?
Found the process name from anyrun.
eqnedt32.exe

## 5. What is the full path of the downloaded payload?
As the payload should be executed, I checked virustotals behavior tab's process tree. 

![](https://i.imgur.com/Jk4c7xx.png)

## 6. Where is the URL used to fetch the payload?

There is only on HTTP request, thats test.png

```
https://raw.githubusercontent.com/accidentalrebel/accidentalrebel.com/gh-pages/theme/images/test.png
```

## 7. What is the flag inside the payload?
From here I couldn't answer any questions as to do further analysis I need to download the `test.png` payload from above link which does not exist at the time I'm doing analysis.