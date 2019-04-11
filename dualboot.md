# Install Dell Laptop as dual boot

## Ubuntu Install Notes
* Install from bootable CD
* Regular Install... Looking to use only half the drive

Procedure
* English
* US
* Keyboard, detect, no
  * English (US)
* Pri Net: enp9s0
* Hostname dellubunutu
* us.archive.ubuntu.com
* No proxy
* Dave
* coateds
* attempts to auto detect timezone
* Partitioning (install to one 40 GB partition, half the available
  * manual
  * delete existing partitions
  * Higlight the free space and enter
  * create a new partition
  * enter a size for the partition, primary, beginning, use as Ext4, done
  * Finish
* (installs stuff)
* No auto updates, no software
* Install grub boot loader (no other OS present)

## CentOS Install Notes
boot from CentOS bootable CD
* English
* Installation Destination... I will configure paritioning
* create mount point automatically (sda2)
* nic must be turned on (set host name too)
* install source: http://mirror.centos.org/centos/7/os/x86_64/
* Software: Minimal
* openssh-server is already installed

## Resultant environment
running `lsblk` in the CentOS environment gives:
```
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0 74.5G  0 disk
├─sda1            8:1    0 37.3G  0 part
├─sda2            8:2    0    1G  0 part /boot
└─sda3            8:3    0 36.3G  0 part
  ├─centos-root 253:0    0 32.6G  0 lvm  /
  └─centos-swap 253:1    0  3.7G  0 lvm  [SWAP]
sr0              11:0    1 1024M  0 rom
```

Where (using df -h commands)
* sda1 is the Ubuntu
  ```
  Filesystem      Size  Used Avail Use% Mounted on
  /dev/sda1        37G  3.2G   32G  10% /
  ```
* sda2 is the boot partition
  ```
  Filesystem      Size  Used Avail Use% Mounted on
  /dev/sda2      1014M  178M  837M  18% /boot
  ```
* sda3 is the CentOS using LVM
  ```
  Filesystem               Size  Used Avail Use% Mounted on
  /dev/mapper/centos-root   33G  1.3G   32G   4% /
  ```

Grub2 boot menu allows either CentOS or Ubuntu to be selected
```
CentOS Linux (3.10.0-957.10.1.el7.x86_64) 7 (Core)
CentOS Linux (3.10.0-957.el7.x86_64) 7 (Core)
CentOS Linux (0-rescue-2a461ef275ad45398c37ed5d4a98a86e) 7 (Core)
Ubuntu 18.04.2 LTS (18.04) (on /dev/sda1)
Advanced options for Ubuntu 18.04.2 LTS (18.04) (on /dev/sda1)
```
Given this, the first and fourth entries are logically the most interesting. Also, logically, the first entry (CentOS) is the default. To make Ubuntu the default (the OS that starts without intervention) the following steps must be taken:
* Boot into CentOS. Since it was installed last, it "owns" the boot menu
* `sudo grub2-set-default 3`
* Note that grub2 commands in RHEL are `grub2-...` and in DEB are `grub-...`

In order to ssh to both CentOS and Ubunutu environments when using the same IP:
* `ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null coateds@192.168.1.171`