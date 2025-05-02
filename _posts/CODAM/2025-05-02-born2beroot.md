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

The hostname of your virtual machine must be your login ending with 42 (e.g., wil42). You will have to modify this hostname during your evaluation.




## Todo

what is apparmor. how to install. how to setup

You must create at least 2 encrypted partitions using LVM. Below is an example of the expected partitioning.

An SSH service will be running on the mandatory port 4242 in your virtual machine. For security reasons, it must not be possible to connect using SSH as root.

You have to configure your operating system with the UFW firewall and thus leave only port 4242 open in your virtual machine.

You have to implement a strong password policy.
-Your password has to expire every 30 days.
- The minimum number of days allowed before the modification of a password will be set to 2.
- The user has to receive a warning message 7 days before their password expires.
- Your password must be at least 10 characters long. It must contain an uppercase letter, a lowercase letter, and a number. Also, it must not contain more than 3 consecutive identical characters.
- The password must not include the name of the user.
- The following rule does not apply to the root password: The password must have at least 7 characters that are not part of the former password.
- Of course, your root password has to comply with this policy.

You have to install and configure sudo following strict rules.
- Authentication using sudo has to be limited to 3 attempts in the event of an incorrect password.
- A custom message of your choice has to be displayed if an error due to a wrong password occurs when using sudo.
- Each action using sudo has to be archived, both inputs and outputs. The log file has to be saved in the /var/log/sudo/ folder.
- The TTY mode has to be enabled for security reasons.
- For security reasons too, the paths that can be used by sudo must be restricted. Example: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin

In addition to the root user, a user with your login as username has to be present.
- This user has to belong to the user42 and sudo groups
- During the defense, you will have to create a new user and assign it to a group.

## Monitoring script

You have to create a simple script called monitoring.sh. It must be developed in bash.

At server startup, the script will display some information (listed below) on all terminals and every 10 minutes (take a look at wall). The banner is optional. No error must be visible.

During the defense, you will be asked to explain how this script works. You will also have to interrupt it without modifying it. Take a look at cron.

our script must always be able to display the following information:
- The architecture of your operating system and its kernel version.
- The number of physical processors.
- The number of virtual processors.
- The current available RAM on your server and its utilization rate as a percentage.
- The current available storage on your server and its utilization rate as a percentage.
- The current utilization rate of your processors as a percentage.
- The date and time of the last reboot.
- Whether LVM is active or not.
- The number of active connections.
- The number of users using the server.
- The IPv4 address of your server and its MAC (Media Access Control) address.
- The number of commands executed with the sudo program.

## Submission and eval

You only have to turn in a signature.txt file at the root of your Git repository.
You must paste in it the signature of your machine’s virtual disk. To get this signature, you first have to open the default installation folder /home/mvan-rij/sgoinfre/Born2beRoot.
Then, retrieve the signature from the ".vdi" file of your virtual machine in sha1 format.
Linux: sha1sum born2beroot.vdi

This is an example of what kind of output you will get:
- 6e657c4619944be17df3c31faa030c25e43e40af

Please note that your virtual machine’s signature may be altered after your first evaluation. To solve this problem, you can duplicate your virtual machine or use save state.{: .prompt-tip }
