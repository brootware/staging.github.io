---
layout: post
title: Fixing Virtualbox lag on macOS Monterey
categories:
- Projects
- Misc
tags:
- Projects
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

![No resolution option](/assets/img/blogImages/noOption.png)

To work around this, you will instead have to edit a variable within the **info.plist** file instead.

![info.plist location](/assets/img/blogImages/infoPlist.png)

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
