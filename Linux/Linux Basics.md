### Edit host names (Ubuntu 18.4)
Configuring the network interface in Ubuntu 18.04
https://serverspace.io/support/help/configuring-the-network-interface-in-ubuntu-18-04/?attempt=1

Checking your IP Address:<br>
``ip addr show``<br>

Change your localhost name:<br>
``sudo vim /etc/hostname``<br>

Update the local and remote host names and IPs:<br>
``sudo vim /etc/hosts``<br>

To check Ethernets and update them:<br>
``sudo vim /etc/netplan/00-installer-config.yaml``<br>
* This is yaml file. Here you can also set static IPs.<br>

To add users:<br>
``sudo adduser username``<br>

To generate an ssh key:<br>
``ssh-keygen -t rsa``<br>

The list of authorized hosts is located:<br>
``~/.ssh/authorized_keys``<br>

To edit the sudoers file:<br>
``sudo visudo``<br>
* To add no password for a sudo user:<br>
``username  ALL=(ALL) NOPASSWD:ALL``<br>

List Block Devices:<br>
``lsblk``<br>
