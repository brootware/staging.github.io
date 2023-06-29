---
layout: post
title: Configuring WSL2 for vagrant and ansible
categories:
- Projects
- Automation
tags:
- Projects
- Automation
- Bash
---

# Introduction
I have recently moved back to using windows as the new apple M1 and M2 chips are not compatible with running vagrant and virtualbox. As someone who likes to run labs locally without breaking the bank spending a lot on Apple hardware. I had to find alternatives and being a mac user since 2018, I was overwhelmed with options avaialble in Windows ecosystem. I came across Lenovo's Thinkpad Z16 which is a very close competitor to build quality of a macbook. So I purchased it over the Christmas prior to the GST(Goods Service Tax) hikes in Singapore.

# Setting up a new machine and automating tasks

Below are the steps in order. Do note that **vagrant versions installed on both Windows Native and WSL2 has to be same** for this to work.
* Enable WSL2 from additional window feature.
* Install Vagrant on Windows and on WSL2 according to your linux distro https://developer.hashicorp.com/vagrant/downloads.
* Check for the version of vagrant installed by running `vagrant -v` in powershell and WSL2.
* Ensure they are the same version.
* If not, go directly to source URL to download the same versions of windows msi and linux package to install locally. https://releases.hashicorp.com/vagrant
  * Windows installation will end with `*.msi` and Ubuntu would be `*.deb`.