---
title: "Lab Server - Serenity"
linkTitle: "Lab Server"
summary: >
  How to access and work with the lab server
---

## Overview
Some useful commands to help navigate and use Serenity

To log into Serenity, you will need to be under the university network or use a [VPN](https://it.ufl.edu/ict/documentation/network-infrastructure/vpn/) provided by the university.

Login in

ssh username@serenity.ifas.ufl.edu

In case of the warnings below

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
```
edit `~/.ssh/known_hosts` and remove the line with serenity

To change your password, use 

`passwd username`

OR

`sudo passwd username`

[To set up ssh keys](/docs/computers-and-programming/ssh/#setup-instructions-serenity--hipergator--generic) 
 
[RStudio on serenity](/docs/computers-and-programming/rstudio-on-serenity/)