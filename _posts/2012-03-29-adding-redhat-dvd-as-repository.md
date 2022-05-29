---
title: Adding RedHat DVD as Repository
date: 2012-03-29 09:00:00 -0800
layout: post
categories: [Linux]
tags: Linux RedHat
---
When we try out a new linux OS its always a pain to download and install all the software and  add packages we needs on that. Its even worse if you don?t have an unlimited connection. But actually most of the software we need is already there in the CD/DVD in which the operating system comes with. So how do we install from the DVD then rather than from the internet?  
For this you need to add the DVD as a repository so that rpm client picks up the rpm from the DVD and wont go to internet for it.  
The Procedure and Files to be modified is a bit different for each OS so i will cover Red Hat Linux in this post.  
In RHEL the repo list is maintained in the folder **/etc/yum.repos.d/** . So lets create a new file in this directory , say lets call it **rhel-cd.repo**.  
The Contents of this file should be as follows:

```
[rhel-cd]
name=Red Hat Enterprise Linux $releasever - $basearch - CD
baseurl=file:///media/RHEL/Server/
enabled=1
gpgcheck=0
```
First line ([rhel-cd]) should be an unique value ie no two repo file should have the same value or it will show a warning.

Name can be anything it is for the user to identify it when its shown though the rpm interface.

baseurl should point to the DVD mount point. For this we first identify the file **repomd.xml**. This file should be in the folder **repodata**. Thus we should include the folder path to the parent folder to repodata in the baseurl.  
Eg: In my DVD path to repomd.xml is /media/RHEL/Server/repodata/repomd.xml , then I include /media/RHEL/Server/ in the baseurl as file:///media/RHEL/Server/

enabled should be 1 if the repo should be taken by rpm on searching for sources.

gpgcheck can be enabled if you have the gpg file for the cd. Lets leave it disabled for now.

Now save the file and close it. So the configuration for adding a new repo is done. If you have any more .repo files in the folder **/etc/yum.repos.d/** then open those and disable ( by making enabled=0) those for now as rpm might choose them over cd repo and it will prompt for downloading from internet. After doing that also we need to clean the cache of yum so that it re-reads the repodata and caches again. This is essential as then only the changes made to .repo files get reflected. You can clean cache by the command

`yum clean all`

Now you can install new rpms from the DVD by using **yum** command.  
So try it out and tell me how it goes and tell me if it doesn?t work or if you need some help

The content of this post is from <http://blog.sriunplugged.com/linux/adding-redhat-dvd-as-repository/>. Want to make sure i give credit to source.
