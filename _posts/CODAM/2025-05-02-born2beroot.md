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

## Born2beRoot
I've written this step by step walktrough for myself. Because after a 3rd reset I didn't want to google the whole setup again. This setup is not guaranteed to get you to pass as you need to demonstrate your abilities and knowledge during the eval.

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



## Install sudo and setup users

### sudo
1. su -> to enter root
2. apt install sudo -> install the program
3. newgrp sudo -> create a new group called sudo
4. sudo adduser mvan-rij sudo -> adds user to sudo
5. groups mvan-rij -> check if sudo is listed

### user42 group
1. sudo groupadd user42 -> adds user42 to /etc/group
2. sudo adduser mvan-rij user42 -> adds mvan-rij to group user42
3. exit -> from root go back to mvan-rij
4. exit -> log out and back in to mvan-rij
5. groups -> sudo and user42 should be in there

## Setup sudo
[custom logfile](https://tverma.hashnode.dev/custom-sudo-logs-file-linux)\
[sudoers manpage](https://linux.die.net/man/5/sudoers)\
[password tries](https://askubuntu.com/questions/534868/how-can-i-change-the-number-of-password-entry-attempts-allowed-by-sudo)

You have to install and configure sudo following strict rules.
- Authentication using sudo has to be limited to 3 attempts in the event of an incorrect password.
- A custom message of your choice has to be displayed if an error due to a wrong password occurs when using sudo.
- Each action using sudo has to be archived, both inputs and outputs. The log file has to be saved in the /var/log/sudo/ folder.
- The TTY mode has to be enabled for security reasons.
- For security reasons too, the paths that can be used by sudo must be restricted. Example: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin

1. mkdir /var/log/sudo -> make folder to put log in
2. touch /var/log/sudo/sudo.log
3. change the sudo config file with: sudo visudo
4. Add the following code to /etc/sudoers.tmp

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

## SSH and firewall
### SSH
An SSH service will be running on the mandatory port 4242 in your virtual machine. For security reasons, it must not be possible to connect using SSH as root.

1. sudo apt install openssh-server -> install ssh
2. sudo service ssh status -> check to make sure it is working
3. sudo nano /etc/ssh/sshd_config -> edit ssh config file

```
Port 4242

PermitRootLogin no
```

4. sudo nano /etc/ssh/ssh_config -> edit another ssh config file

```
Port 4242
```

5. sudo service ssh restart -> for the changes to apply
6. sudo service ssh status -> check to make sure changes are applied

### Firewall (UFW)
You have to configure your operating system with the UFW firewall and thus leave only port 4242 open in your virtual machine.

1. sudo apt install ufw -> install UFW firewall
2. sudo ufw enable -> enable the firewall
3. sudo ufw allow 4242 -> enable port 4242
4. sudo ufw status -> check current UWF status

### Change virtualbox settings
1. open settings for the virtual machine
2. go to the nework tab
3. click the advaced popout
4. click port forwarding
5. set the rules according to the table

| Name   | Protocol | Host IP | Host Port | Guest IP | Guest Port |
|--------|----------|---------|-----------|----------|------------|
| Rule 1 | TCP      |         | 4242      |          | 4242       |

## SSH into the server
1. start the server
2. unlock the encryption
3. open terminal on your main system
4. ssh mvan-rij@localhost -p 4242 -> connect to the server with user mvan-rij on port 4242
5. login
6. you now have a secure remote shell you can do anything on the server with (but with copy paste abilities from your normal system)

## Password policy
[man page](https://linux.die.net/man/8/pam_pwquality)\
[example implementation](https://www.velaninfo.com/rs/techtips/debian-vs-ubuntu-distros/)

1. get pam_pwquality to use the flags: sudo apt install libpam-pwnquality
2. change the expire time and minimum time before modification with: nano /etc/login.defs
3. edit the following lines to:

```
PASS_MAX_DAYS	30
PASS_MIN_DAYS	2
PASS_WARN_AGE	7
```

4. change the password policy file with: nano /etc/pam.d/common-password
5. add the following lines to:

```
#Born2beRoot changes
password	requisite		pam_pwquality.so retry=3 minlen=10 ucredit=-1 lcredit=-1 dcredit=-1 maxrepeat=3 difok=7 reject_username
password	requisite		pam_pwquality.so retry=3 minlen=10 ucredit=-1 lcredit=-1 dcredit=-1 maxrepeat=3 reject_username enforce_for_root
```

6. passwd -> start the password manager to change the password to a new one. mess around with it to check if the rules are applied correctly.

## Set correct timezone and time
1. sudo timedatectl set-timezone Europe/Amsterdam
	1. I got the message: Failed to connect to bus: No such file or directory\
		I fixed this by running the command: sudo service systemd-logind start [found here](https://superuser.com/questions/1561076/systemctl-user-failed-to-connect-to-bus-no-such-file-or-directory-debian-9)

## Monitoring script
You have to create a simple script called monitoring.sh. It must be developed in bash.

At server startup, the script will display some information (listed below) on all terminals and every 10 minutes (take a look at wall). The banner is optional. No error must be visible.

During the defense, you will be asked to explain how this script works. You will also have to interrupt it without modifying it. Take a look at cron.

our script must always be able to display the following information:
- The architecture of your operating system and its kernel version.
	- uname -a
- The number of physical processors.
	- nproc
- The number of virtual processors.
	- nproc --all
- The current available RAM on your server and its utilization rate as a percentage.
	- free
- The current available storage on your server and its utilization rate as a percentage.
	- df -m -x tmpfs
- The current utilization rate of your processors as a percentage.
	- [vmstat](https://linux.die.net/man/8/vmstat)
- The date and time of the last reboot.
	- who -b
- Whether LVM is active or not.
	- lsblk but only get type colmn and grep for lvm there.
	- lsblk -o TYPE
- The number of active connections.
	- ss -Ht state established
- The number of users using the server.
	- users
- The IPv4 address of your server and its MAC (Media Access Control) address.
	- hostname
	- ip link show
- The number of commands executed with the sudo program.
	- cat /var/log/sudo/sudo.log

/usr/local/bin/monitoring.sh

```bash
#!/bin/bash

#System info and kernel version
sysinfo=$(uname -a)

#Number of physical processors
cpucount=$(nproc)

#Number of virtual processors
vcpucount=$(nproc --all)

#Current available RAM and util rate %
ramtotal=$(free --mega | awk ' /^Mem:/ {print $2}')
ramused=$(free --mega | awk ' /^Mem:/ {print $3}')
ramfree=$(free --mega | awk ' /^Mem:/ {print $4}')
rampct=$(awk "BEGIN {print ($ramused / $ramtotal) * 100}")
rampct=$(printf "%.2f%%" "$rampct")

#Current available storage and util rate %
disktotal=$(df -m -x tmpfs | grep '/dev/' | grep -v '/boot' | awk '{disktotal += $2} END {print disktotal}')
diskused=$(df -m -x tmpfs | grep '/dev/' | grep -v '/boot' | awk '{disktotal += $3} END {print disktotal}')
diskpct=$(awk "BEGIN {print ($diskused / $disktotal) * 100}")
diskpct=$(printf "%.2f%%" "$diskpct")

#Current CPU util rate %
cpuidle=$(vmstat 1 2 | tail -1 | awk '{print $15}')
cpuused=$(awk "BEGIN {print (100 - $cpuidle)}")
cpupct=$(printf "%.2f%%" "$cpuused")

#Last boot date and time
boottime=$(who -b | awk '{print $3 " " $4}')

#LVM active?
lvmactive=$(if lsblk -o TYPE | grep -wq "lvm"; then printf "yes"; else printf "no"; fi)

#Number of active connections
nconnections=$(ss -Ht state established | wc -l)

#Number of users using the server
nusers=$(users | wc -w)

#IPv4 adress of server and MAC adress
ipv4=$(hostname -I)
macaddr=$(ip link show | grep "link/ether" | awk '{print $2}')

#Number of commands executed with sudo
nsudo=$(cat /var/log/sudo/sudo.log | wc -l)
nsudo=$(awk "BEGIN {print ($nsudo / 2)}")

wall "
#Architechture:  $sysinfo
#CPU physical:   $cpucount
#vCPU:           $vcpucount
#Memory Usage:   $ramused/${ramfree}MB (${rampct})
#Disk Usage:     $diskused/${disktotal}MB (${diskpct})
#CPU load:       $cpupct
#Last boot:      $boottime
#LVM use:        $lvmactive
#Connections TCP:$nconnections
#User log:       $nusers
#Network:        $ipv4 $macaddr
#Sudo:           $nsudo
"
```

## Crontab
At server startup, the script will display some information (listed below) on all terminals and every 10 minutes.

[crontab tutorial](https://www.geeksforgeeks.org/crontab-in-linux-with-examples/)

1. sudo crontab -u root -e -> edit crontab for user root
2. set to run the monitoring.sh every boot
3. set to run the monitoring.sh every 10 minutes

```
@reboot bash /usr/local/bin/monitoring.sh
*/10 * * * * bash /usr/local/bin/monitoring.sh
```

## Extra info

check for stats in /proc/ -> has a lot of system info in simple text files\
sudo systemctl status cron -> current crontab status and a couple logs\
sudo journalctl -u cron -b -> check cron logs for current boot\
sudo apt install man-db -> install man pages on the server

## Submission and eval
You only have to turn in a signature.txt file at the root of your Git repository.
You must paste in it the signature of your machine’s virtual disk. To get this signature, you first have to open the default installation folder /home/mvan-rij/sgoinfre/Born2beRoot.
Then, retrieve the signature from the ".vdi" file of your virtual machine in sha1 format.
Linux: sha1sum born2beroot.vdi

- During the defense, you will have to create a new user and assign it to a group.

This is an example of what kind of output you will get:
- 6e657c4619944be17df3c31faa030c25e43e40af

<!-- markdownlint-capture -->
<!-- markdownlint-disable -->
> Please note that your virtual machine’s signature may be altered after your first evaluation. To solve this problem, you can duplicate your virtual machine or use save state
{: .prompt-tip }
<!-- markdownlint-restore -->

commands for eval:
- uname -a
- service ufw status
- service ssh status
- adduser test
- addgroup eval
- adduser test eval
- deluser test
- delgroup eval
- visudo
- /etc/pam.d/common-password
- /etc/login.defs
- /etc/hostname
- lsblk
- sudo -V
- sudo ufw
- sudo ufw delete allow 4343
- /usr/local/bin/monitoring.sh
- sudo crontab -u root -e
- sudo journalctl -u cron -b

Current passwords:
- root: Codam2025!
- user mvan-rij: Codam2025!
- drive encription: codam2025
