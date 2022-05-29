---
title: Using DISM to install Storage Drivers
layout: post
categories: [Windows]
---
If you migrate Windows installations between storage adapters, you're often left with the well known STOP 0x7B `INACCESSIBLE_BOOT_DEVICE`.

This happens because Windows doesn't yet have the required drivers installed, and/or set as boot-critical.

The [dism.exe](http://technet.microsoft.com/en-us/library/hh824971.aspx) tool allows us to install (boot-critical) drivers into an offline Windows "image". Note that an offline Windows "image" is nothing special - a regular Windows install is a valid Windows "image".

After a STOP 0x7B, Windows Boot Manager usually sets up fallback boot into [WinRE](http://technet.microsoft.com/en-us/library/cc766048.aspx) (Windows Recovery Environment). WinRE has a copy of the DISM tool, so you're good to go. (Cancel the Startup Recovery assistant if you have to.)

Example DISM command to use from the WinRE (or WinPE) Command Prompt:
```cmd
    dism /image:d:\ /add-driver /driver:e:\ /recurse
```
