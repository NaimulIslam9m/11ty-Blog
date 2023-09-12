---
title: HawkEye Writeup - CyberDefenders
description: An accountant at your organization received an email regarding an
  invoice with a download link. Suspicious network traffic was observed shortly
  after opening the email. As a SOC analyst, investigate the network trace and
  analyze exfiltration attempts.
author: Naimul Islam
date: 2023-08-06T09:15:40.046Z
tags:
  - post
image: https://cyberdefenders.org/media/terraform/HawkEye/HawkEye.jpg
imageAlt: ""
---
> An accountant at your organization received an email regarding an invoice with a download link. Suspicious network traffic was observed shortly after opening the email. As a SOC analyst, investigate the network trace and analyze exfiltration attempts.

## 1. How many packets does the capture have?
```
tshark -r stealer.pcap | wc -l
4003
```
## 2. At what time was the first packet captured?
Go to statistics > capture file properties. First packet capture time 2019-04-10 02:37:07 UTC.
## 3. What is the duration of the capture?
Go to statistics > capture file properties. Elapse time 01:03:41.
## 4. What is the most active computer at the link level?
Go to statistice > endpoint in wireshark. Go to Ethernet tab. Sort the packets column in descending order. First MAC addres is the answer.

00:08:02:1c:47:ae
## 5. Manufacturer of the NIC of the most active system at the link level?
Searching first 3 byte of MAC address in google we found the manufacturer is Hewlett Packard.
## 6. Where is the headquarter of the company that manufactured the NIC of the most active computer at the link level?
Googling Hewlett Packard headquarter we got the name is Palo Alto.
## 7. The organization works with private addressing and netmask /24. How many computers in the organization are involved in the capture?
![](https://i.imgur.com/ekbPi9U.png)

10.4.10.0/24 is local network. Here 10.4.10.255 is broadcast IP. So, there are 3 computers.
## 8. What is the name of the most active computer at the network level?
![](https://imgur.com/nZQ16r0.png)

We can see most active computer at network level is 10.4.10.132. So, using `ip.addr==10.4.10.132 && frame contains -pc` display filter we get 5 packets. Following the tcp stream we get name.

BEIJING-5CD1-PC

## 9. What is the IP of the organization's DNS server?
Using `dns.flags == 0x0100` display filter we see DNS query request is going to `10.4.10.4` IP.

## 10. What domain is the victim asking about in packet 204?
proforma-invoices.com
## 11. What is the IP of the domain in the previous question?
217.182.138.150
## 12. Indicate the country to which the IP in the previous section belongs.
France (used https://whatismyipaddress.com/)
## 13. What operating system does the victim's computer run?
Filter display packet `http` and follow the first packet. There you will get the operating system in user-agent.
Windows NT 6.1
## 14. What is the name of the malicious file downloaded by the accountant?
Going to 'Statistics > IPv4 Statistics > All Addresses' we can see most of the communication is done between two addresses. So we can assume one is accountant (victim) and another is malicious server.

![](https://i.imgur.com/8RTzLN8.png)

Applying `ip.addr==217.182.138.150` display filter we find the following file is downloaded. The file size is quite large and is downloaded by segmented.

tkraw_Protected99.exe

## 15. What is the md5 hash of the downloaded file?
Going to 'File > Export Objects > HTTP' we can export the file. Then applying md5sum we get,
71826ba081e303866ce2a2534491a2f7

## 16. What is the name of the malware according to Malwarebytes?
Google for 'Spyware.HawkEyeKeylogger'
## 17. What software runs the webserver that hosts the malware?
Following 210 number packet.
![](https://i.imgur.com/18kHMOT.png)

## 18. What is the public IP of the victim's computer?
From stream 16
```
250-p3plcpnl0413.prod.phx3.secureserver.net Hello Beijing-5cd1-PC [173.66.146.112]
```
## 19. In which country is the email server to which the stolen information is sent?
Applying `smpt` display filter we get `23.229.162.69` mail server IP.
United States

## 20. Analyzing the first extraction of information. What software runs the email server to which the stolen data is sent?
From stream 16
Exim 4.91

## 21. To which email account is the stolen information sent?
From stream 16
```
echo "c2FsZXMuZGVsQG1hY3dpbmxvZ2lzdGljcy5pbg==" | base64 -d  
sales.del@macwinlogistics.in
```
## 22. What is the password used by the malware to send the email?
From stream 16
```
echo "U2FsZXNAMjM=" | base64 -d  
Sales@23
```
## 23. Which malware variant exfiltrated the data?
From stream 16
```
echo "SGF3a0V5ZSBLZXlsb2dnZXIgLSBSZWJvcm4gdjkgLSBQYXNzd29yZHMgTG9ncyAtIHJvbWFuLm1jZ3VpcmUgXCBCRUlKSU5HLTVDRDEtUEMgLSAxNzMuNjYuMTQ2LjExMg==" | base64 -d  
HawkEye Keylogger - Reborn v9 - Passwords Logs - roman.mcguire \ BEIJING-5CD1-PC - 173.66.146.112
```

Reborn v9
## 24. What are the bankofamerica access credentials? (username:password)
After decoding stream 16 base64 encoded mail.
roman.mcguire:P@ssw0rd$
## 25. Every how many minutes does the collected data get exfiltrated?
After checking mail sending time of stream 16 & 21 found the interval is 10 minutes.