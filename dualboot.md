Ubuntu Install Notes
Regular Install... Looking to use only half the drive

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

boot from CentOS bootable CD
* English
* Installation Destination... I will configure paritioning
* create mount point automatically (sda2)
* nic must be turned on (set host name too)
* install source: http://mirror.centos.org/centos/7/os/x86_64/
* Software: Minimal
* openssh-server is already installed
