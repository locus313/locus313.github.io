---
title: Troubleshooting Locked-out Accounts in a Windows 2008/R2 Domain
layout: post
categories: [Windows]
tags: Windows AD
---
One of my colleagues’ account was constantly being locked out. I suspected that he had used his account to run a service, or other automated task on a server and I needed to find out which one.

As I’d previously used the Microsoft “Account Lockout and Management Tools”, I downloaded the latest version from here (<http://www.microsoft.com/en-gb/download/details.aspx?id=18465>). There are two useful utilities “LockoutStatus.exe”, which shows the state of a specific account on each domain controller (useful to identify which DC is locking out the account) and “eventcombMT.exe” which gathers the event logs from all the DC’s and parses them for specific events.

Although the package runs on 2008 and later OS’ (you need to run it as an administrator, with read access to your domain controller event logs), it only searches for the Event IDs that were valid for Server 2003 and earlier.

Luckily Microsoft has published the new Event IDs for Server 2008 and later (See: Description of security events in Windows Vista and in Windows Server 2008:<http://support.microsoft.com/kb/947226>), and the new event id I required was 4740 (“A user account was locked out”), but I also included 4625 (“An account failed to logon”).

To search for account lockouts with the new event id in EventCombMT:

  1. On the **Searches** menu, point to **Built In Searches**, and then click **Account Lockouts**.  
    All domain controllers for the domain appear in the **Select To Search/Right Click To Add**box. Also, in the** Event IDs** box, you see that event IDs 529, 644, 675, 676, and 681 are added.
  2. In the **Event IDs** box, type a space, and then type **4740 4625** after the last event number.
  3. Click **Search**.

Once the search has completed, you should be presented with the output folder (by default it is in C:\Temp) with two or more small text files with the events listed – these should help you identify which machines are causing the lockout.

source:  
http://3rdlinesupport.wordpress.com/2012/11/03/troubleshooting-locked-out-accounts-in-a-windows-2008r2-domain/
