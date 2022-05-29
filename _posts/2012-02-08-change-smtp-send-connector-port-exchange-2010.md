---
title: Change smtp send connector port exchange 2010
date: 2012-02-08 09:00:00 -0800
layout: post
categories: [Exchange 2010]
tags: Exchange
---
If you need to change the default smtp send connectory port you can run the following command:
```powershell
 Set-SendConnector -Identity "Default" -port 26
```
Then you will need to restart the Microsoft Exchange Transport for the change to take effect.
