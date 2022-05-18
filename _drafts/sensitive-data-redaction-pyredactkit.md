---
layout: post
title: Sensitive data redaction tool - PyRedactKit 🧰🔐📝
categories: [Projects, security-toolkit]
tags: [Projects, security-toolkit, Python, Data-redaction, data-security, log-redaction]
---

# Introduction

There are a lot of open-source cyber security tools that help automate a lot of the tasks on the Red team/PT side of things like domain enumeration, network scanning, etc. When I searched for a Blue team/defense-focused tool for security and privacy especially redacting data, I could only find a few of them on Github. The PyRedactKit is an inspiration and expansion of Ben's original [redact-py CLI tool](https://github.com/ben-labs/redact-py) that redacts IP addresses from log files.

PyRedactKit aims to sanitize and redact Personally Identifiable Information (PII) such as Names, Email addresses, Credit Card, SG NRIC and other sensitive data like internal IP addresses and domain names. One great use case of the tool would be if you are working in a SOC and you have to send some log data to a 3rd party for troubleshooting or whatever reason, PyRedactKit can come in hand to redact those sneaky dns and ip addresses.

# First implementation

First implementation of the PyRedactKit was straightforward. I took inspiration from a couple of projects involving  It uses a library called commonregex, an open source project by Madison May on github.

# Bottle necks and slow performance

## Single-file processing bottle neck

```bash
➜  PyRedactKit git:(600fc8f) ✗ time python pyredactkit.py ip_test.txt                                                               
 
    ______       ______         _            _     _   ___ _   
    | ___ \      | ___ \       | |          | |   | | / (_) |  
    | |_/ /   _  | |_/ /___  __| | __ _  ___| |_  | |/ / _| |_ 
    |  __/ | | | |    // _ \/ _` |/ _` |/ __| __| |    \| | __|
    | |  | |_| | | |\ \  __/ (_| | (_| | (__| |_  | |\  \ | |_ 
    \_|   \__, | \_| \_\___|\__,_|\__,_|\___|\__| \_| \_/_|\__|
           __/ |                                               
           |___/                                                                                                           
            +-+-+-+-+-+-+-+ +-+-+ +-+-+-+-+-+-+-+-+-+
            |P|o|w|e|r|e|d| |b|y| |B|r|o|o|t|w|a|r|e|
            +-+-+-+-+-+-+-+ +-+-+ +-+-+-+-+-+-+-+-+-+                                                                             
     
[ + ] Processing starts now. This may take some time depending on the file size. Monitor the redacted file size to monitor progress
[ + ] No option supplied, will be redacting all the sensitive data supported

[ + ] Redacted 10021 targets...
[ + ] Took 38.12691903114319 seconds to execute
python3 pyredactkit.py ip_test.txt  67.39s user 0.19s system 99% cpu 1:08.05 total
```

## Multi-file processing bottle neck

```bash
➜  PyRedactKit git:(main) time python pyredactkit.py multiredact -d redacted_dir

    ______       ______         _            _     _   ___ _   
    | ___ \      | ___ \       | |          | |   | | / (_) |  
    | |_/ /   _  | |_/ /___  __| | __ _  ___| |_  | |/ / _| |_ 
    |  __/ | | | |    // _ \/ _` |/ _` |/ __| __| |    \| | __|
    | |  | |_| | | |\ \  __/ (_| | (_| | (__| |_  | |\  \ | |_ 
    \_|   \__, | \_| \_\___|\__,_|\__,_|\___|\__| \_| \_/_|\__|
           __/ |                                               
           |___/                                                                                                           
            +-+-+-+-+-+-+-+ +-+-+ +-+-+-+-+-+-+-+-+-+
            |P|o|w|e|r|e|d| |b|y| |B|r|o|o|t|w|a|r|e|
            +-+-+-+-+-+-+-+ +-+-+ +-+-+-+-+-+-+-+-+-+
            
    https://github.com/brootware
    https://brootware.github.io                                                                             
    
[ + ] redacted_dir directory does not exist, creating it.
[ + ] Processing starts now. This may take some time depending on the file size. Monitor the redacted file size to monitor progress
[ + ] No option supplied, will be redacting all the sensitive data supported

[ + ] Redacted 10038 targets...
[ + ] Took 10.545741081237793 seconds to execute
[ + ] Redacted results saved to redacted_dir/redacted_ip_test 2.txt
[ + ] Processing starts now. This may take some time depending on the file size. Monitor the redacted file size to monitor progress
[ + ] No option supplied, will be redacting all the sensitive data supported

[ + ] Redacted 10038 targets...
[ + ] Took 10.369873046875 seconds to execute
[ + ] Redacted results saved to redacted_dir/redacted_ip_test 4.txt
[ + ] Processing starts now. This may take some time depending on the file size. Monitor the redacted file size to monitor progress
[ + ] No option supplied, will be redacting all the sensitive data supported

[ + ] Redacted 10038 targets...
[ + ] Took 10.294179677963257 seconds to execute
[ + ] Redacted results saved to redacted_dir/redacted_ip_test 3.txt
[ + ] Processing starts now. This may take some time depending on the file size. Monitor the redacted file size to monitor progress
[ + ] No option supplied, will be redacting all the sensitive data supported

[ + ] Redacted 10038 targets...
[ + ] Took 10.296481847763062 seconds to execute
[ + ] Redacted results saved to redacted_dir/redacted_ip_test.txt
python3 pyredactkit.py multiredact -d redacted_dir  42.12s user 0.20s system 100% cpu 41.941 total
```

# Optimizing and refactoring code for speed

# Implementing Unredaction function

# Automating CICD

# Todos and enhancements

- [x] Refactor regex functions as a class?
- [x] Singapore NRIC
- [x] Further performance improvement, 10s -> 1.7s for over 4GB data.
- [x] Tokenization for unredacting data
- [x] Base64 supoort
- [ ] Multiprocessing files
