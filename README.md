# VagrantWindowsOnLinux
Notes and Research to run a Vagrant Windows 10 installation on a Linux box

Using an old Dell Laptop with Ubuntu (at first) to work out procedures for installing a Windows 10 Vagrant/VirtualBox

The dellubuntu.md file contains notes about installing Ubuntu on the laptop

Projects to work on:
* Linux
  * Samba (for editing files over net connection with VSCode)
  * grub practice
    * press/hold [esc] to display grub boot menu on a UEFI system ([shift] for BIOS system)
  * dual boot with CentOS?
  * Git Prompt
    * Add to .bashrc
    ```
    parse_git_branch() {
         git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
    }
    export PS1="\u@\h \[\033[32m\]\w\[\033[33m\]\$(parse_git_branch)\[\033[00m\] $ "
    ```
    * https://askubuntu.com/questions/730754/how-do-i-show-the-git-branch-with-colours-in-bash-prompt
* Next subject

