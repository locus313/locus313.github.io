---
title: Copying mailbox content to a subfolder in another mailbox
layout: post
categories: [Exchange 2010]
---
This is the command to export the content from Tim?s mailbox to a folder name ?Tim? in Tom?s mailbox:
```powershell
Export-Mailbox -Identity <tim@example.com> -TargetMailbox <tom@example.com> -TargetFolder tim
```
