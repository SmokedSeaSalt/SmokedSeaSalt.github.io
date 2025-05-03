---
title: Born2beRoot
description: My Born2beRoot notes.
author: Mathijs
date: 2025-05-02 8:03:00 +0100
categories: [CODAM]
tags: []
pin: false
math: false
mermaid: true
---

## Virtualbox setup
Downloaded iso from [Debian](https://www.debian.org/)

importand settings in new:
- virtual machine name and operating system
	- folder = sgoinfre
	- add iso file
	- skip unattended installation marked
- Virtual hard disk
	- set drive to 8GB. bigger makes sha1sum and cloning longer.
	- create a Virtual hard disk now
	- check pre-alloate full size

## Debian setup

The hostname of your virtual machine must be your login ending with 42 (e.g., wil42). You will have to modify this hostname during your evaluation.

install steps:
1. language: do whatever (but english is handy)
2. location: just do US. time can be changed later.
3. keyboard: American English.
4. hostname: mvan-rij42
5. domain name: empty
6. root password: codam2025 (for now)
	1. retype same password
7. new user fullname: mvan-rij
8. new user account name: mvan-rij
9. user password: codam2025 (for now)
	1. retype same password
10. timezone: just select eastern (will be changed later)
11. parition disk: Guided - use entire disk and set up encrypted LVM
12. what disk: the only option -> VBOX HARDDISK
13. paritioning scheme: Seperate /home folder
	1. this will create the 2 encrypted folder needed.
14. are you sure?: write changes -> yes
15. encryption passphrase: codam2025
	1. retype same password
16. amount of volume: max
17. finish partitioning
18. write changes to disk? -> yes
19. scan extra installation media? -> no
20. debian archive mirror country: Netherlands
21. debian archive mirror: deb.debian.org
22. HTTP proxy: nothing
23. participate in package usage survey?: no
24. choose software to install: unselect everyting (using space)
25. install GRUB boot loader?: yes
26. Device for boot loader installation: VBOX_HARDDISK
27. installation complete: Continue

Current passwords:
- root: codam2025
- user mvan-rij: codam2025
- drive encription: codam2025

## install sudo and setup users

### sudo
su -> to enter root
apt install sudo -> install the program
newgrp sudo -> create a new group called sudo
sudo adduser mvan-rij sudo -> adds user to sudo
groups mvan-rij -> check if sudo is listed

### user42 group

sudo groupadd user42 -> adds user42 to /etc/group
sudo adduser mvan-rij user42 -> adds mvan-rij to group user42

exit -> from root go back to mvan-rij
exit -> log out and back in to mvan-rij

groups -> sudo and user42 should be in there

## setup sudo

[custom logfile](https://tverma.hashnode.dev/custom-sudo-logs-file-linux)
[sudoers manpage](https://linux.die.net/man/5/sudoers)
[password tries](https://askubuntu.com/questions/534868/how-can-i-change-the-number-of-password-entry-attempts-allowed-by-sudo)

You have to install and configure sudo following strict rules.
- Authentication using sudo has to be limited to 3 attempts in the event of an incorrect password.
- A custom message of your choice has to be displayed if an error due to a wrong password occurs when using sudo.
- Each action using sudo has to be archived, both inputs and outputs. The log file has to be saved in the /var/log/sudo/ folder.
- The TTY mode has to be enabled for security reasons.
- For security reasons too, the paths that can be used by sudo must be restricted. Example: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin

change the sudo config file with: sudo visudo
Add the following code

/etc/sudoers.tmp
```
#Born2beRoot changes
# change log folder
Defaults	logfile="/var/log/sudo/sudo.log"
# set amount of password tries
Defaults	passwd_tries=3
# set new message on failed password
Defaults	badpass_message="Nope, not this time!"
# require the user to only use TTY
Defaults	requiretty
# set paths to be used by sudo
Defaults	secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
```
## Password policy
[man page](https://linux.die.net/man/8/pam_pwquality)
[example implementation](https://www.velaninfo.com/rs/techtips/debian-vs-ubuntu-distros/)
google for reject_username

change the password policy file with: nano /etc/pam.d/passwd
add the following code



You have to implement a strong password policy.
-Your password has to expire every 30 days.
- The minimum number of days allowed before the modification of a password will be set to 2.
- The user has to receive a warning message 7 days before their password expires.
- Your password must be at least 10 characters long. It must contain an uppercase letter, a lowercase letter, and a number. Also, it must not contain more than 3 consecutive identical characters.
- The password must not include the name of the user.
- The following rule does not apply to the root password: The password must have at least 7 characters that are not part of the former password.
- Of course, your root password has to comply with this policy.




## Todo

AppArmor for Debian must be running at startup
- what is apparmor
	user privilages manager
- how to install
- how to setup


An SSH service will be running on the mandatory port 4242 in your virtual machine. For security reasons, it must not be possible to connect using SSH as root.

You have to configure your operating system with the UFW firewall and thus leave only port 4242 open in your virtual machine.





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
	- ? /proc/stat  ->  proccesor stats
	- top row cpu is all cpu's combined.
	- cpu user, nice, system, idle, iowait, irq, softirq.
	- to get current percentage = ((user + system) / (user + system + idle)) * 100
- The date and time of the last reboot.
	- who -b
- Whether LVM is active or not.
	-lsblk but only get type colmn and grep for lvm there.
	-lsblk -o TYPE
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

<!-- markdownlint-capture -->
<!-- markdownlint-disable -->
> Please note that your virtual machine’s signature may be altered after your first evaluation. To solve this problem, you can duplicate your virtual machine or use save state
{: .prompt-tip }
<!-- markdownlint-restore -->
