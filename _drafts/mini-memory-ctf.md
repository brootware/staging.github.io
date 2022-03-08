---
layout: post
title: Mini Memory CTF 🕵️ 💻 
categories: [CTF, Forensics]
tags: [CTF, Memory-Forensics, DFIR, Blue-team]
---
## Category

Forensics

## Challenge Details

This Mini Memory CTF contest has ended, but you can still play! This is an excellent opportunity to get some hands-on practice with memory forensics. You'll find the questions below, as well as a link to download the memory sample needed to answer those questions. When you've completed the challenge, download the solutions guide to check your work.

***If you enjoy this video, please consider supporting 13Cubed on Patreon at patreon.com/13cubed.***

👉 Memory Sample
<https://drive.google.com/drive/folders/1E-i2RTUBXBGUd_Xz0k67kFOpHcr6WX8J>

👉 Mini Memory CTF Solutions Guide
<https://www.13cubed.com/downloads/mini_memory_ctf_solutions_guide.pdf>

## Solution

### Question #1

Find the running rogue (malicious) process. The flag is the MD5 hash of its PID.

First we will find out the profile info of the memory dump using imageinfo.

```bash
$ vol.py -f memdump.mem imageinfo
Volatility Foundation Volatility Framework 2.6.1
INFO    : volatility.debug    : Determining profile based on KDBG search...


          Suggested Profile(s) : Win10x64_17134, Win10x64_14393, Win10x64_10586, Win10x64_16299, Win2016x64_14393, Win10x64_17763, Win10x64_15063 (Instantiated with Win10x64_15063)
                     AS Layer1 : SkipDuplicatesAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/cases/minictf/memdump.mem)
                      PAE type : No PAE
                           DTB : 0x1ad002L
                          KDBG : 0xf800ca7bd520L
          Number of Processors : 1
     Image Type (Service Pack) : 0
                KPCR for CPU 0 : 0xfffff800c9714000L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2018-08-06 18:13:42 UTC+0000
     Image local date and time : 2018-08-06 14:13:42 -0400
```

```bash
$ vol.py -f memdump.mem --profile Win10x64_17134 psscan | grep svchost.exe
Volatility Foundation Volatility Framework 2.6.1
Offset(P)          Name                PID   PPID PDB                Time created                   Time exited                   
------------------ ---------------- ------ ------ ------------------ ------------------------------ ------------------------------

0x0000a780001d6080 svchost.exe        5048    804 0x000000003c400002 2018-08-01 19:21:00 UTC+0000                                 
0x0000c20c6a5514c0 svchost.exe        8808    804 0x0000000079000002 2018-08-06 18:12:05 UTC+0000                                 
0x0000c20c6aa0d580 svchost.exe        8052    804 0x00000000a5c09002 2018-08-06 18:12:40 UTC+0000                                 
0x0000c20c6aaf9080 svchost.exe        1992    804 0x0000000006500002 2018-08-06 18:12:01 UTC+0000                                 
0x0000c20c6ab2b580 svchost.exe.ex     6176   4824 0x000000004d100002 2018-08-01 19:52:19 UTC+0000   2018-08-01 19:52:19 UTC+0000  
0x0000c20c6ab70080 svchost.exe        8852   4824 0x0000000096f00002 2018-08-01 19:59:49 UTC+0000   2018-08-01 20:00:08 UTC+0000  
0x0000c20c6b4c6080 svchost.exe        5048    804 0x000000003c400002 2018-08-01 19:21:00 UTC+0000                                 
0x0000c20c6b513580 svchost.exe        5264    804 0x00000000b8950002 2018-08-01 19:21:11 UTC+0000                                 
0x0000c20c6b585580 svchost.exe        3224    804 0x0000000078e00002 2018-08-01 19:43:30 UTC+0000                                 
0x0000c20c6b5b6580 svchost.exe        4040    804 0x00000000b9770002 2018-08-01 19:21:04 UTC+0000                                 
0x0000c20c6b6a5580 svchost.exe        2020    804 0x0000000023b00002 2018-08-01 19:20:54 UTC+0000                                 
0x0000c20c6b6b5580 svchost.exe        4304    804 0x000000002d400002 2018-08-01 19:20:55 UTC+0000                                 
0x0000c20c6b6c3580 svchost.exe        4132    804 0x0000000028b00002 2018-08-01 19:20:54 UTC+0000                                 
0x0000c20c6b8dd580 svchost.exe         924    804 0x000000010e410002 2018-08-01 19:20:28 UTC+0000                                 
0x0000c20c6b8df580 svchost.exe         904    804 0x000000010ba10002 2018-08-01 19:20:28 UTC+0000                                 
```

Most Parent Process ID has 804 but there are some with sketchy extensions and 4824. We can cross reference by grepping for process id with 4824

```bash
$ vol.py -f memdump.mem --profile Win10x64_17134 psscan | grep 4824
Volatility Foundation Volatility Framework 2.6.1
0x0000c20c69cfe580 explorer.exe       4824   4756 0x0000000035800002 2018-08-01 19:20:58 UTC+0000                                 
0x0000c20c6a959580 FTK Imager.exe     3328   4824 0x000000005dd00002 2018-08-06 18:13:14 UTC+0000                                 
0x0000c20c6ab2b580 svchost.exe.ex     6176   4824 0x000000004d100002 2018-08-01 19:52:19 UTC+0000   2018-08-01 19:52:19 UTC+0000  
0x0000c20c6ab70080 svchost.exe        8852   4824 0x0000000096f00002 2018-08-01 19:59:49 UTC+0000   2018-08-01 20:00:08 UTC+0000  
0x0000c20c6ab92580 ByteCodeGenera     6532   4824 0x000000004c200002 2018-08-01 19:50:42 UTC+0000   2018-08-01 19:50:42 UTC+0000  
0x0000c20c6abeb580 notepad.exe        1412   4824 0x0000000056000002 2018-08-06 18:12:15 UTC+0000   2018-08-06 18:12:17 UTC+0000  
0x0000c20c6b588580 ie4uinit.exe       5716   4824 0x00000000bc500002 2018-08-01 19:21:30 UTC+0000   2018-08-01 19:21:31 UTC+0000  
0x0000c20c6c095580 MSASCuiL.exe       6268   4824 0x000000009ad00002 2018-08-01 19:21:56 UTC+0000                                 
0x0000c20c6cdf4580 scvhost.exe         360   4824 0x000000006af00002 2018-08-01 19:56:45 UTC+0000   2018-08-06 18:12:03 UTC+0000  
0x0000c20c6cfb1580 OneDrive.exe       2200   4824 0x00000000ba600002 2018-08-01 19:22:10 UTC+0000                                 
0x0000c20c6cfc2580 vmtoolsd.exe       3372   4824 0x0000000097700002 2018-08-01 19:21:56 UTC+0000                                 
0x0000c20c6d0d2080 Bubbles.scr       10204   4824 0x0000000047700002 2018-08-01 19:50:33 UTC+0000   2018-08-01 19:50:38 UTC+0000  
```

As we can see from the scan 4824 belongs to explorer.exe which has nothing to do with svchost.

```bash
$ vol.py -f memdump.mem --profile Win10x64_17134 psscan | grep -i svchost | grep 4824
Volatility Foundation Volatility Framework 2.6.1
Offset(P)          Name                PID   PPID PDB                Time created                   Time exited                   
------------------ ---------------- ------ ------ ------------------ ------------------------------ ------------------------------
0x0000c20c6ab2b580 svchost.exe.ex     6176   4824 0x000000004d100002 2018-08-01 19:52:19 UTC+0000   2018-08-01 19:52:19 UTC+0000  
0x0000c20c6ab70080 svchost.exe        8852   4824 0x0000000096f00002 2018-08-01 19:59:49 UTC+0000   2018-08-01 20:00:08 UTC+0000  
0x0000c20c6d5ac340 svchost.exe.ex     5528   4824 0x0000000119400002 2018-08-01 19:52:20 UTC+0000   2018-08-01 19:52:20 UTC+0000  
0x0000c20c6d6fc580 svchost.exe       10012   4824 0x0000000136200002 2018-08-01 19:49:19 UTC+0000   2018-08-01 19:49:19 UTC+0000  
0x0000c20c6d82e080 svchost.exe        1404   4824 0x00000000a0f00002 2018-08-01 19:54:55 UTC+0000   2018-08-01 19:56:35 UTC+0000  
0x0000c20c6d99b580 svchost.exe.ex     8140   4824 0x00000000b8600002 2018-08-01 19:52:16 UTC+0000   2018-08-01 19:52:16 UTC+0000  
0x0000c20c6dbc5340 svchost.exe        7852   4824 0x000000003ff00002 2018-08-01 19:49:21 UTC+0000   2018-08-01 19:49:22 UTC+0000  
0x0000c20c6ddad580 svchost.exe        8560   4824 0x00000000b2200002 2018-08-01 20:13:10 UTC+0000                                 
```

By narrowing down on svchost with PPID 4824 and cross checking on the time exited we can see that there is a svchost process with PID 8560 still running which is most likely the running rogue process.

md5 hash of process id is as below

```bash
echo -n 8560 | md5sum 
bc05ca60f2f0d67d0525f41d1d8f8717  -
```

### Question #2

Find the running rogue (malicious) process and dump its memory to disk. You'll find the 32-character flag within that process's memory.

### Question #3

What is the MAC address of this machine's default gateway? The flag is the MD5 hash of that MAC address in uppercase with dashes (-) as delimiters. Example: 01-00-A4-FB-AF-C2.

### Question #4

Find the full path of the browser cache created when an analyst visited "www.13cubed.com." The path will begin with "Users\." Convert the path to uppercase. The flag is the MD5 hash of that string.
