# dellubuntu
attempt to install Ubuntu to an old Dell laptop

## hostname: dellubuntu
* Install Ubuntu on Dell laptop
* be connected to network
* Pri Net iface: enp9s0
* Dave Coate
* coateds
* grub2, install to MBR

## Apps to install
* vim, openssh-server (required in order to connect from another machine)
* sudo apt install openssh-server
* sudo apt install virtualbox -y
* sudo apt install vagrant
  * default installs 2.02 ubuntu version
  * need at least 2.03 due to conflict
  * 2.04 available
* sudo apt install git -y

## Usage
* Hostkey + F inside the Windows (Virtual Box) window will toggle full screen
* Hostkey is [right ctrl]


## Vagrantfile for Windows box
```
Vagrant.configure(2) do |config|
  config.vm.guest = :windows
  # config.vm.communicator = :winrm
  config.winrm.username = "IEUser"
  config.winrm.password = "Passw0rd!"
  config.vm.box = "Microsoft/EdgeOnWindows10"
  config.vm.box_version = "1.0"
  config.vm.provider :virtualbox do |vb|
    vb.gui = true
    vb.customize ["modifyvm", :id, "--vram", "128"]  # vid RAM
    vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
    vb.memory = 2048
    vb.cpus = 1
  end
end
```
