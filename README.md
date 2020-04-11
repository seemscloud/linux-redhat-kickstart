# Kickstart

## DHCP
```bash
inst.ks=http://10.0.0.2/ks.cfg ip=dhcp
```

## Static ip (old)
```bash
inst.ks=http://10.0.0.2/ks.cfg ip=10.0.0.3 gw=10.0.0.1 netmask=255.0.0.0
```

## Static ip (new)
```bash
inst.ks=http://10.0.0.2/ks.cfg ip=10.0.0.3::10.0.0.1:255.0.0.0
```

## ks.cfg
```bash
auth --enableshadow --passalgo=sha512
cdrom
# graphical
firstboot --enable
ignoredisk --only-use=sda
keyboard --vckeymap=us --xlayouts='us'
lang en_US.UTF-8
eula --agreed
 
services --disabled="chronyd"
timezone Europe/Warsaw --isUtc --nontp
 
network --hostname=pre-pdc-xcr-ide1
 
firewall --disabled
selinux --disabled
 
reboot
 
url --url=http://10.0.0.2/iso/rhel6
 
rootpw --iscrypted $6$7I3T9uXT2eYswvkS$skBKEzBS2Tf7ISV8gNdMEkK7.7/92LCe/ew1HCmahdFw3eBl/wey.Fip/JOZsylI8P8TPnSwfVaJLU4AgElzP.
 
bootloader --location=mbr --boot-drive=sda
clearpart --all --initlabel
 
part /boot --fstype="ext4" --size=1024 --ondisk=sda
 
part pv.00 --size=1 --grow --ondisk=sda
volgroup rhel pv.00
 
logvol / --fstype="ext4" --vgname="rhel" --size=8191 --name="root"
logvol /var --fstype="ext4" --vgname="rhel" --size=8191 --name="var"
logvol /tmp --fstype="ext4" --vgname="rhel" --size=8191 --name="tmp"
logvol /opt --fstype="ext4" --vgname="rhel" --size=8191 --name="opt"
logvol /home --fstype="ext4" --vgname="rhel" --size=8191 --name="home"
logvol /var/log --fstype="ext4" --vgname="rhel" --size=8191 --name="var_log"
logvol /var/tmp --fstype="ext4" --vgname="rhel" --size=8191 --name="var_tmp"
logvol /var/log/audit --fstype="ext4" --vgname="rhel" --size=8191 --name="var_log_audit"
logvol swap --vgname="rhel" --size=8191 --name="swap"
 
%packages
@^minimal
@core
%end
 
%packages
yum-utils
zip
tree
bc
net-tools
make
gcc
chrony
telnet
bind-utils
%end
 
%addon com_redhat_kdump --disable --reserve-mb='auto'
 
%end
 
%post
cat > /root/.bash_profile << 'EndOfMessage'
# Sources
if [ -f ~/.bashrc ] ; then
    . ~/.bashrc
fi
 
# Variables
#PATH=$PATH
 
# Settings
#umask 0022
 
# Exports
#export PATH
EndOfMessage
 
cat > /root/.bashrc << 'EndOfMessage'
# Sources
 
# Variables
PS1="[\[\e[31m\]\u\[\e[m\]][\l]@[\[\e[1;34m\]\h\[\e[m\]][\[\e[1;36m\]\W\[\e[m\]]# "
HISTTIMEFORMAT="%Y-%m-%d %T "
 
# Aliases
alias ls='ls --color=auto --hide=".*"'
alias rm='rm -iv'
alias cp='cp -iv'
alias mv='mv -iv'
alias rmdir='rmdir -v'
 
# Exports
export PS1
export HISTTIMEFORMAT
EndOfMessage
%end
 
%anaconda
pwpolicy root --minlen=6 --minquality=1 --notstrict --nochanges --notempty
pwpolicy user --minlen=6 --minquality=1 --notstrict --nochanges --emptyok
pwpolicy luks --minlen=6 --minquality=1 --notstrict --nochanges --notempty
%end
```
