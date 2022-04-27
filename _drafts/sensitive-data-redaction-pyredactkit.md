---
layout: post
title: Sensitive data redaction tool - PyRedactKit 🧰🔐📝
categories: [Projects, security-toolkit]
tags: [Projects, security-toolkit, Python, Data-redaction, data-security, log-redaction]
---

# Introduction

There are a lot of open-source cyber security tools that help automate a lot of the tasks on the Red team/PT side of things like domain enumeration, network scanning, etc. When I searched for a Blue team/defense-focused tool for security and privacy especially redacting data, I could only find a very few on Github. The PyRedactKit is an inspiration and expansion of Ben's original [redact-py CLI tool](https://github.com/ben-labs/redact-py) that redacts IP addresses from log files.

PyRedactKit aims to sanitize Personally Identifiable Information (PII) such as Names, Email addresses, Credit Card, SG NRIC and other sensitive data like internal IP addresses and domain names. The

# First implementation

# Bottle necks and slow performance

# Optimizing and refactoring code for speed

# Todos and enhancements

- [ ] Refactor regex functions as a class?
- [ ] Singapore NRIC
- [ ] Multiprocessing files
