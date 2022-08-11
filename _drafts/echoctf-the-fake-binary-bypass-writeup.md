---
layout: post
title: 'EchoCTF: The Fake Binary Bypass Writeup ⚙️ 🛠'
categories:
- CTF
- Binary-Exploitation
tags:
- CTF
- Binary-Exploitation
---

## Summary

It has been a while last I did an online CTF. So this entry is a refresher on my knowledge of binary analysis.

We are given a binary file to analyze and find the flags. The challenges were pretty simple. Simple enumerations on the given binary file would lead you to flags. So let's jump in!

### Ident flag from this binary

Lets start with the easy part, analyze the binary and answer what was the hostname of the system this binary was compiled on?

Very first important thing when it comes to binary analysis challenges is to check if there are any unstripped strings in the binary. You can simply use `strings` command for this.

```bash
$ strings -a binary_analysis_bypass | grep ETSCTF
ETSCTF_%x
ETSCTF_%x%x
diz b1n4ry fil3 w4s compiled on ETSCTF_{redacted}
ETSCTF_{redacted}.{redacted}
ETSCTF
```

From this first enumeration we already got the hostname of the system and the complete source code filename that produced this binary.

### Linux distro and version this binary was compiled on

```bash
$ file binary_analysis_bypass 
binary_analysis_bypass: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), statically linked, for GNU/Linux 3.2.0, BuildID[sha1]=89047470b27fed12ce5e60b0b1c55e528c2e461f, not stripped
```

From checking out the file type of the binary we can see that this is most likely compiled using gnu compiler. We can try to look for `gcc` in the strings output of the binary to try to find out linux distro and version.

```bash
$ strings -a binary_analysis_bypass | grep gcc   
DW.ref.__gcc_personality_v0
.gcc_except_table

$ strings binary_analysis_bypass | grep GCC   
GCC: (redacted) 8.3.0
```

### Name of the default function being called by main()

When the program is run it executes a single function from its main(). Analyze the binary and answer what is the name of that function.

We will need ghidra to do static code analysis for this binary.
First find out the main function from the ghidra UI as below.

![main](https://bn1304files.storage.live.com/y4msnt64-G42WH27Ngr9HKUBp8XiS0BYuBWNa3dYrHN1UJ3JES8aqD5osvtvvmjudI8pH-R0ZAGAIt0VXUWi2QEW54_4UHXvJ4FYEunBjFmhG-DhQ8aSlVjJC7DsqJNBqfgjnQSeLscTAjerHMRLGsx7sGmt6f6Q7Kf4HPWO-ziP8WdS--Bl9UVEKJHpCYMPS1k?width=492&height=1660&cropmode=none)

Click into the main function and you will be able to see the function that is being called in the main function.

![functioncall](https://bn1304files.storage.live.com/y4mhfkj0Z0rFL4rcXsZCspL2e_8HruaOdt7_wpyoWl804R9i98FRHu83HQ0ZlAsEcx8chPutTwqo3nKiLjOHEZ5dZieRqVbrg7QtTpeH4UZsxOafkhYyiA0x7iaU4t9dmQ_uEtCLsRI-7WFFnikNHCRSW354tsMaQe4A8a_ptaHsUDbNG9fDYhuqHDaFv1LFpkL?width=2858&height=1492&cropmode=none)

What would be the flag being displayed when the program is run?

```bash
$ ./binary_analysis_bypass 
ETSCTF_{redacted}
```
