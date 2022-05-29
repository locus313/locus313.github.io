---
title: 3750 interface bandwidth limiting
layout: post
categories: [Network]
tags: Network Cisco
---
I want to police customers traffic into 20mbps.

**Ingress policing**  
Create policy map:
```
policy-map shape-20  
class class-default  
police 20M 400000 exceed-action drop  
```
Assign policy map to interface:
```
interface FastEthernet1/0/2  
service-policy input shape-20
```
**Egress policing**  
Unfortunately, policy-map containing police action cannot be attached to interface in egress direction. So here is how i limit it to 20mbps:
```
interface FastEthernet1/0/2  
srr-queue bandwidth limit 20  
srr-queue bandwidth shape 0 0 0 0
```
