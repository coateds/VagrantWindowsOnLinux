# Windows Vagrant Box on Linux provisioned with Chef via winrm

Still amazed I got this to work even partially. It took a lot of trying different things, so I am not completely sure what was needed

Links:
* I found a few things here, but not sure any of it relevant, these lines were added to the Vagrantfile
* https://github.com/hashicorp/vagrant/issues/6430
  * config.winrm.retry_limit = 30
  * config.winrm.retry_delay = 10
* Changing network connections from Public to Private
* https://answers.microsoft.com/en-us/windows/forum/windows_10-networking-winpc/windows-10-cannot-change-network-from-public-to/b46fbd08-1e63-4bb3-b1d3-b8d391f7ebf6
* This must be done for all connections
```
# Open Windows PowerShell in admin mode. Start -> PowerShell -> Right-click -> open as administrator
# Get current profiles. Make sure you are logged on to the network you want to change. 
Get-NetConnectionProfile
# Change the network in your list to be private
Set-NetConnectionProfile -Name "MYWIFINETWORK" -NetworkCategory Private
# Check that everything went fine
Get-NetConnectionProfile
```
* Most useful page:
* http://www.hurryupandwait.io/blog/understanding-and-troubleshooting-winrm-connection-and-authentication-a-thrill-seekers-guide-to-adventure
* winrm must be configured on the client `winrm quickconfig`
* troubleshoot the connection from the Linux host `nc -z -w1 <IP or host name> 5985;echo $?`
* allow through the windows firewall
  * for testing, I just turned the fire wall off
  * `netsh advfirewall firewall add rule name="WinRM-HTTP" dir=in localport=5985 protocol=TCP action=allow`
  * `netsh advfirewall firewall add rule name="WinRM-HTTPS" dir=in localport=5986 protocol=TCP action=allow`
* Accessing via cross platform tools like chef, vagrant, packer, ruby or go? Run these commands:
```
winrm set winrm/config/client/auth '@{Basic="true"}'
winrm set winrm/config/service/auth '@{Basic="true"}'
winrm set winrm/config/service '@{AllowUnencrypted="true"}'
```
* Vagrant docs for winrm configurations:  https://www.vagrantup.com/docs/vagrantfile/winrm_settings.html 


Vagrantfile
```
Vagrant.configure(2) do |config|
  config.vm.guest = :windows
  config.vm.communicator = :winrm
  config.winrm.username = "IEUser"
  config.winrm.password = "Passw0rd!"
  config.winrm.retry_limit = 30
  config.winrm.retry_delay = 10
  config.vm.box = "Microsoft/EdgeOnWindows10"
  config.vm.box_version = "1.0"
  config.vm.network "private_network", type: "dhcp"
  config.vm.provider :virtualbox do |vb|
    vb.gui = true
    vb.customize ["modifyvm", :id, "--vram", "128"]  # vid RAM
    vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
    vb.memory = 2048
    vb.cpus = 1
  end

  config.vm.provision "chef_zero" do |chef|
    chef.cookbooks_path = "cookbooks"
    chef.data_bags_path = "data_bags"
    chef.nodes_path = "nodes"
    chef.roles_path = "roles"

    chef.add_recipe "windows-as-code::default"

  end
end
```