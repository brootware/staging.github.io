---
layout: post
title: Fixing Virtualbox lag on macOS Monterey
categories:
- General-tech
- Virtualization
tags:
- General-tech
- Virtualization
- VirtualBox
date: 2022-02-15 13:48 +0800
---
## Introduction

If you are using Mac and running Linux virtual machines on virtual box, you might have experienced slowness and lags compared to using VMWare Fusion. This blog is a quick fix to this problem.

## Why it happens

There are no official statements from virtual box addressing the problem directly. Only bits and pieces of information around the community forums and blogs provide a clue on [lack of support for retina 4k display](https://forums.virtualbox.org/viewtopic.php?f=8&t=90446).

## How to resolve

After looking around for resolution, I come across a forum post to run Virtualbox in low resolution as per the screenshot below.

![Virtualbox forum](https://forums.virtualbox.org/download/file.php?id=37813)

Here are the steps to resolution.

1. Navigate to Apps folder. Choose VirtualBox.app
2. Right-click on VirtualBox.app, Show Package Contents.
3. Contents -> Resources -> VirtualBoxVM (right-click -> Get info)
4. Check the "Open in Low Resolution" checkbox.
5. Run the Virtual Machine in 100% scale mode.

## Wait, I can't find the Open in Low-Resolution option anymore

Indeed, in newer macOS Monterey version 12 and above you will not be able to find that option anymore following the above steps for the solution.

![No resolution option](https://bn1304files.storage.live.com/y4mfskAMaYclH70GOsk4-fQiLx-SrM1Et5DaB01grh_qWnDuQ7ZokhpCQXiC_zLhDwoOB6P08zr_ZR923_VlEwf-MNYP5YWopL2BmhcrfKT9NpnHhTD9bNFN2uc8LPd1EDwncb6vIZclOTNXDFAghUQDEKMTgSxONxuninpL4F2RvYTpAHaXoDGWGusWiibuq7Rsit8RdR6x_Ap8t_2GLJwyJC47SxNL6RrtoRsc2mwT2M?encodeFailures=1&width=2047&height=1502)

To work around this, you will instead have to edit a variable within the **info.plist** file instead.

![info.plist location](https://bn1304files.storage.live.com/y4mhOyMdoQUUBo3r7v9bYQlwKrsR2JGQdrkCoPS3YhKF3XrMAWGaP-PeSz2Ww4_LqekuTAklPYch034T3opNmGpg2p92WAP-SYlSVPp87aa7jLdWsOs-xIMK29uRneZia7IlIo-ljZ4lj3RHcc03aAriXC2qPl_pzGwqEvgDkhXloRXZRL8ctOgX2J-wgScr-0yvcVqXrmHFHda2AoOVei5xucPK7afyEJaqSM599AxV9o?encodeFailures=1&width=964&height=478)

As a sudoer (Admin user), you will need to edit this variable from

```xml
<key>NSHighResolutionCapable</key> <true/>
```

to

```xml
<code>NSHighResolutionCapable</key> <false/>
```

You can either use vi or vscode to edit this.

To edit from the terminal with vi, run the following command.

```bash
sudo vi /Applications/VirtualBox.app/Contents/Resources/VirtualBoxVM.app/Contents/Info.plist
```

To edit using vscode. (You will be prompted for admin password)

```bash
sudo code /Applications/VirtualBox.app/Contents/Resources/VirtualBoxVM.app/Contents/Info.plist
```

Search for `NSHighResolutionCapable` within the XML file and change the value to false. This fix has worked well for me for my labs bringing up and destroying virtual machines with vagrant and Virtualbox.
