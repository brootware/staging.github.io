---
layout: post
title: Sensitive data redaction/unredaction tool - PyRedactKit 🧰🔐📝
categories: [Projects, security-toolkit]
tags: [Projects, security-toolkit, Python, Data-redaction, data-security, log-redaction]
---

# Introduction

There are a lot of open-source cyber security tools that help automate a lot of the tasks on the Red team/PT side of things like domain enumeration, network scanning, etc. When I searched for a Blue team/defense-focused tool for security and privacy especially redacting data, I could only find a few of them on Github. The PyRedactKit is an inspiration and expansion of Ben's original [redact-py CLI tool](https://github.com/ben-labs/redact-py) that redacts IP addresses from log files.

PyRedactKit aims to sanitize and redact Personally Identifiable Information (PII) such as Names, Email addresses, Credit Card, SG NRIC and other sensitive data like internal IP addresses and domain names. One great use case of the tool would be if you are working in a SOC and you have to send some log data to a 3rd party for troubleshooting or whatever reason, PyRedactKit can come in hand to redact those sneaky dns and ip addresses.

# First implementation

First implementation of the PyRedactKit was straightforward. I took inspiration and used libraries from a couple of open source projects  [commonregex](https://github.com/madisonmay/CommonRegex), an open source project by Madison May on github. There are simple wrapper functions that calls the commonregex library to search for the sensitive data in a text file. It worked great for a start.

But as I ran it on bigger and bigger log files with a few hundred thousands of records, the run time suffered.

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

For a single file with over 12k lines of records, it took over a minute to complete. If we pay attention to the last two lines, there were 2 parts that were taking half a minute each.

```console
[ + ] Took 38.12691903114319 seconds to execute
python3 pyredactkit.py ip_test.txt  67.39s user 0.19s system 99% cpu 1:08.05 total
```

The first part was on this [particular function](https://github.com/brootware/PyRedactKit/blob/600fc8f65d629560608fc436f61614bc74a427e8/src/redact.py#L174).

```python
    def to_redact(self, data=str, redact_list=[]):
        redact_count = 0
        start = time.time()

        for elm in redact_list:
            total_elm = len(elm)
            # encode element to be blocked
            elm = r'\b' + elm + r'\b'
            # multiply the block with length of identified elements
            bl = total_elm * self.block
            # substitute the block using regular expression
            data = re.sub(elm, bl, data)
            redact_count += 1

        end = time.time()
        print()
        print(f"[ + ] Redacted {redact_count} targets...")
        time_taken = end - start
        print(f'[ + ] Took {time_taken} seconds to execute')
        return data
```

The function was simply iterating through a list of identified strings returned from the commonregex regular expression library and redacting them from the text files. In terms of time complexity this was linear time increase directly proportional to number of elements in the list. O(n)

If we were to run this for multiple log files with hundred thousands of records, the redaction will take much longer.

A solution was considered to refactor the function to use binary search instead of iterative linear search. However, as the list could not really be sorted I had to come up with alternative solution.

An alternative to this is to not store the identified regex strings in a list but rather redact them on the fly. In order to achieve that I had to stop using the commonregex library by madison may and build the regex library from scratch.

So a separate class for identifier is created to maintain a database of regular expressions with data type for redaction. This made the project extensible and much cleaner. Contributors can also extend the type of data they want to redact just by modifying this particular [regex library](pyredactkit/identifiers.py).

With these new and better implementations, I was able to reduce the time from 67 seconds to a mere 1 second for a single file redaction.

```bash
pyredactkit.py ip_test.txt  1.74s user 0.13s system 124% cpu 1.504 total
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

# Automating CICD

# Implementing Unredaction function

# Reporting function to show man hour saved

# Todos and enhancements

- [x] Refactor regex functions as a class?
- [x] Singapore NRIC
- [x] Further performance improvement, 10s -> 1.7s for over 4GB data.
- [x] Reporting function to show how much hours you have saved by using the tool.
- [x] Tokenization for unredacting data
- [x] Base64 supoort
- [ ] Multiprocessing files
