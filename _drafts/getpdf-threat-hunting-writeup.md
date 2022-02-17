---
layout: post
title: GetPDF Threat Hunting writeup
categories: [CTF, Forensics, Threat-Hunting]
tags: [CTF, Malware-Analysis, Incident-Response, Blue-team]
---
Point: 1200

<https://cyberdefenders.org/blueteam-ctf-challenges/progress/rootware/84/>

## Category

Forensics, Threat-Hunting, Malware-Analysis

## Solution

### How many URL path(s) are involved in this incident?

```text
http.request.method == GET
```

![urls](/assets/img/blogImages/getPDF1.png)
There are altogether 6 unique URLs after analyzing the pcap file in wireshark.
Answer: 6

### What is the URL which contains the JS code?

![jsurl](/assets/img/blogImages/getPDF2.png)
So the full URL path that contains the JS code is
<http://blog.honeynet.org.my/forensic_challenge/>
