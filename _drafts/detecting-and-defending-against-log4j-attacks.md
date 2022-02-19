---
layout: post
title: Detecting and defending against log4j attacks
categories:
- CTF
- Forensics
tags:
- CTF
- Windows-Forensics
- Log4j
- Log4shell
---
Point: 2000

<a href="https://cyberdefenders.org/blueteam-ctf-challenges/progress/rootware/73/"><img src="https://img.shields.io/badge/completed-lespion%20opensource%20intelligence-brightgreen.svg" /></a>

## Category

Forensics

## Challenge Details

For the last week, log4shell vulnerability has been gaining much attention not for its ability to execute arbitrary commands on the vulnerable system but for the wide range of products that depend on the log4j library. Many of them are not known till now. We created a challenge to test your ability to detect, analyze, mitigate and patch products vulnerable to log4shell.

## Solution

### 1. What is the computer hostname?

The computer hostname can be found in the network setup log file which is recording the domain join activity. The path to the file is `C:\Windows\debug\NetSetup.LOG`

![netsetup](/assets/img/blogImages/log4j/log4jq1.png)

hostname: VCW65

## 2. What is the Timezone of the compromised machine?

Using Autopsy, you are able to view registry files through `SYSTEM` registry in the following path:

`C:\Windows\System32\config\SYSTEM`

Open the `SYSTEM` registry file > go to ControlSet001 > Control > TimeZoneInformation > TimeZoneKeyName

![timezone](/assets/img/blogImages/log4j/log4jq2.png)

`Pacific Standard Time`

We can see that the time zone is in PST. By default PST and UTC have difference of 8 hours

Hence `UTC-8`

### 3. What is the current build number on the system?

The current windows build number can be found via `SOFTWARE` registry within Autopsy via this path.

`C:\Windows\System32\config\SOFTWARE`

Open the `SOFTWARE` registry file > Microsoft > Windows NT > CurrentVersion

![buildversion](/assets/img/blogImages/log4j/log4jq3.png)

The current build number is 14393.

### 4. What is the computer IP?

The IP address of the computer can be found in previous `SYSTEM` registry as follow.

Open the `SYSTEM` registry file > go to ControlSet002 > Services > Tcpip > Parameters > Interfaces > `{82e9...}`

![ipaddress](/assets/img/blogImages/log4j/log4jq4.png)

The IP address is 192.168.112.139

### 5. What is the domain computer was assigned to?

The domain of the computer can be found in previous `SYSTEM` registry as follow.

Open the `SYSTEM` registry file > go to ControlSet002 > Services > Tcpip > Parameters

![domain](/assets/img/blogImages/log4j/log4jq5.png)

cyberdefenders.org
