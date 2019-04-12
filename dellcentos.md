# notes for configuring a centos box after installation second to dual boot system

Setup the gui
* yum groupinstall -y 'gnome desktop'
* yum install -y 'xorg*'
* yum remove -y initial-setup initial-setup-gui
* systemctl set-default graphical.target
* systemctl isolate graphical.target

yum update

Install VirtualBox
* yum install -y kernel-devel kernel-headers gcc make perl
* wget https://www.virtualbox.org/download/oracle_vbox.asc
* rpm --import oracle_vbox.asc
* wget http://download.virtualbox.org/virtualbox/rpm/el/virtualbox.repo -O /etc/yum.repos.d/virtualbox.repo
* yum install -y VirtualBox-6.0

Run VBoxManage --version to see if all OK, might need some of these commands:
* sudo /sbin/vboxconfig
* sudo yum install -y kernel-devel kernel-devel-3.10.0-957.el7.x86_64
* /sbin/vboxconfig

Install vagrant 2.2.4
* wget https://releases.hashicorp.com/vagrant/2.2.4/vagrant_2.2.4_x86_64.rpm
* yum localinstall vagrant_2.2.4_x86_64.rpm