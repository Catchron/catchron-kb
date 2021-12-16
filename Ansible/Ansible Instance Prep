## VirtualBox with Ubuntu 18.04.6 LTS
### Configure VirtualBox and the VMs for SSH
Configuring the VirtualBox Network so that ssh works between the VMs in VirtualBox.
We are using 2 network Adapters in this case:
* NAT adapter – for our Virtual Machine to access the Internet
* Host-only adapter – for us to access the VM locally by SSH, HTTP, etc.

1. Add a NAT Network:<br>
* In VBox go to **File** >> **Host Network Manager**
* Make sure there is a **Host-Only Ethernet adapter** with **DHCP** setup
* Note the **Host Network IP**
2. Configure the NAT Adapter for the Virtual Machine
* Select your VM and go to **Settings** >> **Network** >> **Adapter 1**
  * Make sure it is set to **NAT**
* Go to **Adapter 2**
  * Make sure it is set to **Host-Only Adapter**

2. Set a Static IP in Ubuntu for your VMs in VirtualBox
* Check the Host Network IP
* We need to configure our VMs with Static IP from the same range.
  * Example: If our network has an IP of ``192.168.56.1/24`` then our first host would be ``192.168.56.2`` and the next host would be ``192.168.56.3``
* To set the static IP we will need to configure the network interfaces from a yaml file.
* Edit the following yaml file:<br>
``sudo vim /etc/netplan/00-installer-config.yaml``
* Add the following info for 192.168.56.2:<br>
```
network:
  ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
      dhcp4: false
      addresses: [192.168.56.2/24]
  version: 2
	```

* Add the static IP to the hosts file:<br>
``sudo vim /etc/hosts``
* Add the IP followed by the host name:<br>
``192.168.56.2 anscontrol``

3. Reboot the system:<br>
``sudo reboot``

4. Try to SSH to your VM using PuTTY and the static IP you have just created.

5. Do the same procedure but with different static IPs for the other VMs in VirtualBox

### Create the Ansible Users
Now that you have the VMs setup - we need to add the Ansible Users

1. To add a new **ans** user:<br>
``sudo adduser ans``

2. Edit the sudoers file to allow the **ans** user to elevate permissions:<br>
* Edit the sudoers file using:
``sudo visudo``<br>
or
``vim /etc/sudoers``<br>

* Add the following line:<br>
``username  ALL=(ALL) NOPASSWD:ALL``<br>

3. Do this for all hosts involved with Ansible

### Create and Transfer SSH keys

After the users have been created we need to create and transfer the SSH keys between the Ansible Control node and the Ansible Hosts so that ssh is seamless and without a password.

1. Create the SSH key on the Ansible Control:
ssh-keygen -t rsa (accept all defaults)

2. Vie the generated key and copy it:<br>
``cat .ssh/id_rsa.pub``

3. On your Ansible Host create a new directory in the **ans** user's home directory for the authorized keys:<br>
``mkdir .ssh``

4. Create the authorized_keys file:<br>
``touch .ssh/authorized_keys``

5. Copy the public key you created for Anisble Control in the authorized_keys file with vim:<br>
``vim .ssh/authorized_keys``

6. Once this is done you should be able to ssh between the Ansible Control and the Ansible Host without password prompt.

### Install Ansible on the Ansible Control

Install **Ansible on Debian Systems**:<br>
``sudo apt update``<br>
``sudo apt install software-properties-common``<br>
``sudo add-apt-repository --yes --update ppa:ansible/ansible``<br>
``sudo apt install ansible``<br>

### Update the hosts template for Ansible

1. Edit the hosts file:<br>
``sudo vim /etc/ansible/hosts``

2. Add your inventory of hosts:<br>

```
[anshosts]
anshost1 ansible_host=192.168.56.3
[localhost]
192.168.56.2
```
