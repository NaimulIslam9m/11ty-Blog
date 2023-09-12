---
title: Exploring the Wide Range of Projects Provided by abuse.ch
description: A brief description of different projects of abuse.ch
author: Naimul Islam
date: 2022-12-31T09:49:48.535Z
tags:
  - post
image: https://images.unsplash.com/flagged/photo-1560854350-13c0b47a3180?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwzMDAzMzh8MHwxfHNlYXJjaHwxfHxtYWx3YXJlfGVufDB8fHx8MTY4MTA4ODMyMw&ixlib=rb-4.0.3&q=80&w=1080
imageAlt: ""
---
[abuse.ch](https://abuse.ch/) is providing *community driven* threat intelligence on cyber threats with a strong focus on malware and botnets. It is a collection of following projects:

### MalwareBazaar

[MalwareBazaar](https://bazaar.abuse.ch/) is a place where your can get *malware samples* which is shared by malware researchers for infosec community.

*What it offers?*

* Latest & Old malware samples.

### Feodo Tracker

[Feodo Tracker](https://feodotracker.abuse.ch/) is place where you can get latest malicious *botnet C2 servers* IP associated with Emotet, Dridex, TrickBot, QakBot and BazarLoader.

*What it offers?*

* Latest IP addresses of botnet C2 servers (Set of infected computers).
* Latest IOCs (Indicators Of Compromise) of C2 servers. Use it with your SIEM for more accurate detection.
* Latest Ruleset for **Suricata** & **Snort** to detect and/or block network connections toward C2 servers.

### SSL Blacklist

[SSL Blacklist (SSLBL)](https://sslbl.abuse.ch/) detects malicious SSL connections, by identifying and blacklisting SSL certificates used by C2 servers. It also identifies *JA3 fingerprints* that helps you to detect & block malware C2 communication on the *TCP layer*.

*What it offers?*

* List of *SHA1 fingerprints* of malicious SSL certificates.
* Get list of *JA3 fingerprint* to find malware in your network.
* SSL Certificate & JA3 Ruleset for **Suricata**.

### ThreatFox

[ThreatFox](https://threatfox.abuse.ch/) shares indicators of compromise (IOCs) associated with malware with the infosec community.

*What it offers?*

* A structured database for IOCs.
* Search IOCs of different threat using different operations. For example: `malware:CobaltStrike`, `threat_type:cc_skimming`, etc.

### YARAify

[YARAify](https://yaraify.abuse.ch/) allows anyone to scan *suspicious files* such as *malware samples* or *process dumps* against a large repository of YARA rules.

*What it offers?*

* Scan suspicious file with [ClaimAV](https://www.clamav.net/) antivirus.
* Unpack PE executables or DLL files.
* YARAhub, a structured database for yara rules.
* Search yara rule using different operators. For example: `yara:MALWARE_Win_Zegost`, `md5:bf130acead582841f356719dd6d29e98`, etc.

### URLhaus

[URLhaus](https://urlhaus.abuse.ch/) shares *malicious URLs* that are being used for malware distribution. Its Malware Database gives you the latest data on domains registered as phishing or as spam with the goal of sharing malicious URLs that are being used for malware distribution.

*What it offers?*

* Updated database for malicious URLs
* It generates a [ClamAV](https://www.clamav.net/) signature database which gets updated **once per minute** for real time detection.
* Search malicious URL using different operators. For example: `filetype:doc`, `tag:elf` etc.

All these 6 abuse.ch project provides API for automating the process. Maybe use these for your next project?