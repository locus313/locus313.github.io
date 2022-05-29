---
title: Show Command Multiple Filtering
layout: post
categories: [Network]
tags: Network Cisco
---
Normally when we do show command we make use of the `|` to filter and put in keywords after like include, exclude, begin and section. As we all know "include" means show only that matches the string like for the example below.
```
R1#sh run | inc CISCO 
neighbor CISCO peer-group
```

We can do some multiple command filtering like the example below using the "include" keyword. Let&#8217;s say we want to see the interface name, then the description, the OSPF cost and if its configured with the "mpls ip" command.
```
R1#sh run | inc interface |^ description |^ ip ospf cost |^ mpls ip
interface FastEthernet0/0
description towards LAN
ip ospf cost 100
mpls ip
```

The trick is to use multiple `|` and then the regular expression `^`. Then put a space before the string because the configurations under the interface configuration if you do a `show run` has a space before the line. This also applies to the "exclude" keyword but who the heck uses "exclude" that much?


Source:

http://ciscodreamer.blogspot.com/2009/08/multiple-command-filtering.html
