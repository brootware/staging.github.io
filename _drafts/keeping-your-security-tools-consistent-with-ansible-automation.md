---
layout: post
title: Keeping your security tools consistent with ansible automation
date: 2022-02-11 08:13 +0800
categories: [Projects, kali-up]
tags: [Projects, security-toolkit, Ansible, Automation]
---
As security professionals, it could be very time consuming to spin up a kali machine with all the tools needed to do any form of Red or Blue team work like bug bounty, threat hunting and malware analysis. To embark on this project, here is a checklist of things borrowed from Infrastructure-As-Code philosophy from DevOps to fit my needs. The automation tool has to be:

- Portable across platform. It has to be distro agnostic, able to run anywhere: A workstation or a vm running Linux. 
- Has to be extensible. More tools can be added and more customization can be done like adding vagrant and a provisioner to spin up multiple vms.
- Idempotent to easily manage versioning of security testing tools within the automation.


So I looked around for any open source projects that might fit this need and come across a couple of interesting projects which is [disposable-kali](https://github.com/stevemcilwain/Disposable-Kali) by stevemcilwain and a project called [kali-up](https://github.com/archcloudlabs/kali-up) by archcloudlabs. 

[disposable-kali](https://github.com/stevemcilwain/Disposable-Kali) project did not meet the three points mentioned earlier as the project is running a bash script to setup all the tools which is also distro specific (ubuntu) and not able to run on any other distros like Fedora or Suse.

[kali-up](https://github.com/archcloudlabs/kali-up) project was fairly simple when I discovered. The setup is done via ansible roles which fits all three requirements. Each ansible roles contain a collection of security testing tools that gets installed.

- [x] Portable
- [x] Extensible
- [x] Idempotent

You simply have to clone the repo and run `ansible-playbook site.yml` to install all the tools locally. But I ran into a fair bit of errors as some of the roles have broken links for installation.

So I forked the repo and added in further customization to fit my needs.