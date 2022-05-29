---
title: Putty Auto Login Via Command Line
layout: post
categories: [SSH]
---
So lets say you want to open a new PuTTY session to a certain IP Address and have it automatically enter your username for you.
```cmd
C:\putty.exe username@192.168.0.1
```
Now lets do the same thing but with a password too.
```cmd
C:\putty.exe username@192.168.0.1 -pw password
```
The two above are fine if you don't need to use any of the provided settings you get from within PuTTY (e.g. Tunnels, Proxy's, Color Schemes etc) but if you're like me and you need to use Tunnels to tunnel into a server using a number of ports then the simplest way to do is this to create a new PuTTY session and then use the following parameters.
```cmd
C:\putty.exe -load &#8220;Session Name&#8221; -l username -pw password
```
