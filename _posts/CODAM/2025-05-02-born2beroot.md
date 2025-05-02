---
title: Born2beRoot
description: My Born2beRoot notes.
author: Mathijs
date: 2025-05-02 8:03:00 +0100
categories: [CODAM]
tags: []
pin: false
math: false
mermaid: false
---

## Virtualbox setup
Downloaded iso from [Debian](https://www.debian.org/)

importand settings in new:
- virtual machine name and operating system
	- folder = sgoinfre
	- add iso file
	- skip unattended installation marked
- Virtual hard disk
	- create a Virtual hard disk now
	- check pre-alloate full size

## Debian setup

The hostname of your virtual machine must be your login ending with 42 (e.g.,
wil42). You will have to modify this hostname during your evaluation.




## Todo

what is apparmor. how to install. how to setup

You must create at least 2 encrypted partitions using LVM. Below is an example of the
expected partitioning.

An SSH service will be running on the mandatory port 4242 in your virtual machine.
For security reasons, it must not be possible to connect using SSH as root.

You have to configure your operating system with the UFW firewall and thus leave only port 4242 open in your virtual machine.

You have to implement a strong password policy.
-

You have to install and configure sudo following strict rules.
-
