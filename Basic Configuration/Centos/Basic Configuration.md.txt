#Basic Configuration

## Get DHCP
```
[gemaadhan@localhost ~]$ sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3
```
change these parameters
```
BOOTPROTO=DHCP
DHCP=yes
```
```
[gemaadhan@localhost ~]$ sudo ifdown enp0s3 && sudo ifup enp0s3
```

## SET STATIC IP
```
[gemaadhan@localhost ~]$ sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s3
```
change these parameters
```
BOOTPROTO=none
#DHCP=yes
IPADDR=192.168.1.15
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
```
```
[gemaadhan@localhost ~]$ sudo ifdown enp0s3 && sudo ifup enp0s3
```

## ADD USER AND SET AS SUDO MEMBER
```
sudo useradd nadia
sudo passwd nadia
sudo usermod -aG wheel nadia
```