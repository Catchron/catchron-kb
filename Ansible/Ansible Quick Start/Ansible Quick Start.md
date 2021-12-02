# Ansible Quick Start
* [Ansible Introduction and Architecture](https://github.com/Catchron/catchron-kb/blob/main/Ansible/Ansible%20Quick%20Start/Ansible%20Quick%20Start.md#ansible-introduction-and-architecture)
* [Ansible Configuration and Installation](https://github.com/Catchron/catchron-kb/blob/main/Ansible/Ansible%20Quick%20Start/Ansible%20Quick%20Start.md#ansible-configuration-and-installation)
  * [SSH Considerations](https://github.com/Catchron/catchron-kb/blob/main/Ansible/Ansible%20Quick%20Start/Ansible%20Quick%20Start.md#ssh-considerations)
* [Ansible Ad-hoc commands](https://github.com/Catchron/catchron-kb/blob/main/Ansible/Ansible%20Quick%20Start/Ansible%20Quick%20Start.md#ansible-ad-hoc-commands)
* [Ansible Playbooks](https://github.com/Catchron/catchron-kb/blob/main/Ansible/Ansible%20Quick%20Start/Ansible%20Quick%20Start.md#ansible-playbooks)
* [Ansible Variables](https://github.com/Catchron/catchron-kb/blob/main/Ansible/Ansible%20Quick%20Start/Ansible%20Quick%20Start.md#ansible-variables)
* [Ansible facts](https://github.com/Catchron/catchron-kb/blob/main/Ansible/Ansible%20Quick%20Start/Ansible%20Quick%20Start.md#ansible-facts)
* [Troubleshooting and debugging](https://github.com/Catchron/catchron-kb/blob/main/Ansible/Ansible%20Quick%20Start/Ansible%20Quick%20Start.md#troubleshooting-and-debugging)
* [Ansible Handlers](https://github.com/Catchron/catchron-kb/blob/main/Ansible/Ansible%20Quick%20Start/Ansible%20Quick%20Start.md#ansible-handlers)

## Ansible Introduction and Architecture

Ansible is an automation engine that allows for agentless system configuration and deployment.

Ansible operates **over SSH** and runs **Ansible Modules** on remote systems to complete tasks.

Ansible is typically **installed** on a single, lightweights **control node** where a list of host **inventory files** and **playbooks** are kept.

The heavy lifting is generally performed on the **remote host** as that is where **Ansible Modules** are executed.

<img src="https://miro.medium.com/max/512/0*Sr1T30hc8WT269jV" width="500"/><br>

## Ansible Configuration and Installation

For Ansible documentation check: https://docs.ansible.com/

In order to install Ansible you must configure the **EPEL repository (RedHat Systems)** on your system. Once the EPEL repo is configured, your package manager installs Ansible and manages dependencies.

* What is [EPEL](https://docs.fedoraproject.org/en-US/epel/)
 * Extra Packages for Enterprise Linux (EPEL)
 * Extra Packages for Enterprise Linux (or EPEL) is a Fedora Special Interest Group that creates, maintains, and manages a high quality set of additional packages for Enterprise Linux, including, but not limited to, Red Hat Enterprise Linux (RHEL), CentOS and Scientific Linux (SL), Oracle Linux (OL).


* Install **Ansible on Red-Hat Systems**:<br>
``sudo yum install ansible``<br>
and<br>
``sudo yum install git``<br>


* Install **Ansible on Debian Systems**:<br>
``sudo apt update``<br>
``sudo apt install software-properties-common``<br>
``sudo add-apt-repository --yes --update ppa:ansible/ansible``<br>
``sudo apt install ansible``<br>

The primary Ansible configuration file is located in ``/etc/ansible/ansible.cfg``. Notable configurations there include:<br>
 * ``Default inventory configuration``
 * ``Default remote user``

The Default Ansible Inventory File is ``/etc/ansible/hosts``. An inventory lis a list of hosts that Ansible manages.
* The inventory location may be specified a couple of ways:
 * By Default: ``/etc/ansible/hosts``
 * Can be specified by CLI: ``ansible -i``
 * Can be set in ``/etc/ansible/ansible.cfg``

>The **[hosts]()** file contains instructions on how to list you available hosts that can be manipulated by Ansible. You can input your list of hosts at the end of the file. You can simply input the hosts IP addresses as a list at end. For our example we will add the hosts and put them behind a variable so we dont have to type the IP every time when we reference the host:<br>
``anshost1 ansible_host=192.168.56.3``<br>
``anshost1`` :variable name:<br>
``ansible_host=`` :this assoicates the variable to a host <br>
``192.168.56.3`` :host IP or FQDN<br>

>Example:

```
...
## www[001:006].example.com

# Ex 3: A collection of database servers in the 'dbservers' group:

## [dbservers]
##
## db01.intranet.mydomain.net
## db02.intranet.mydomain.net
## 10.25.1.56
## 10.25.1.57

# Here's another example of host ranges, this time there are no
# leading 0s:

## db-[99:101]-node.example.com
anshost1 ansible_host=192.168.56.3
```

> After you've setup the hosts file you will be able to reference **anshost1** when you are using Ansible commands.

### SSH Considerations

> Before we even begin you will have to make sure that you can ssh between your hosts without any issues.
In my case I have two virtual machines in [VirtualBox](https://www.virtualbox.org/) running [Ubuntu Server 20.04 LTS](https://ubuntu.com/download/server). One is called **Ansible Control** and the other is called **Ansible Host 1**. I used this **[guide](https://www.wpdiaries.com/virtualbox-for-web-development/)** to setup SSH between those two VMs in Virtual Box and the [2.2 Configure the Network on NAT + Host-only Adapters](https://www.wpdiaries.com/virtualbox-for-web-development/#network-on-2-adapters) method. Keep in mind that in earlier Ubuntu Versions the **Static IP** setup is different.
Once you've setup **SSH** between your Virtual Machines (or Hosts) you can begin setting up Ansible.

It is possible to connect to a remote host with Ansible via password authentication manually using ``-k``. This however is not a common practice as it defeats the purpose of Ansible automation by having manual intervention.

The best way to implement and use Ansible is by having a common user created across all systems controlled by Ansible. You need to make sure that this common user can **ssh** between the hosts without the need for password authentication. To do that we will need to generate an **ssh key** and shared that key across the ans users in the hosts so that authentication is automatic. This common user must also be added to the ``/etc/sudoers`` file so that it can execute root commands without a password prompt.

> In my case I created a user called **ans** across both the **Ansible Control** and **Ansible Host 1** virtual machines. I added this user with the ``sudo adduser ans`` command for **Ubuntu**. Once I added the user on both machines I had to create an **SSH key** on the **Ansible Control** vm **(with the test user)** and shared the **public** part of that key with the **test user** on the **Ansible Host 1**:
```
ans@ansiblecontrol:~$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/test/.ssh/id_rsa):
Created directory '/home/test/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/test/.ssh/id_rsa
Your public key has been saved in /home/ans/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:ZkF6cpNFvoKwBm6hviYaDEdo++kXGbvAeqWohW2Kqts test@ansiblecontrol
The key's randomart image is:
+---[RSA 3072]----+
|        ..o      |
| .     o +       |
|..+ . o * .      |
|.+.o + = o .     |
|o.= o = S .      |
|+=.+.= o .       |
|oo*o+ o          |
|oXoo o           |
|/oE..            |
+----[SHA256]-----+
```
After the key was generated I copied the key to the **Ansible Host 1** vm using the **ssh-copy-id** command and the IP address of the vm. Keep in mind that the same user must exist on both machines in order for this to work.
```
ans@ansiblecontrol:~$ ssh-copy-id 192.168.56.3
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/ans/.ssh/id_rsa.pub"
The authenticity of host '192.168.56.3 (192.168.56.3)' can't be established.
ECDSA key fingerprint is SHA256:LZ4wtPL8cLRlCFPyCR+n73JtpxELvTqmL3mDMTENQmU.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
test@192.168.56.3's password:
Number of key(s) added: 1
Now try logging into the machine, with:   "ssh '192.168.56.3'"
and check to make sure that only the key(s) you wanted were added.
```
This command will add the public key for ``ans@ansiblecontrol`` to ``/home/test/.ssh/authorized_keys`` for ``ans@ansiblehost1`` and you will be able to ssh with the ans user between those machines without providing a password.

> Once this is done you must add the ans user on your **Ansible Host 1** to the **sudoers** file and make sure it can execute root commands without password. To do that you must add the ``ans ALL=(ALL) NOPASSWD: ALL`` to the ``sudoers`` file located in ``/etc/sudoers``. Add the line in the ``# User privilege specification`` field as seen below:<br>
<img src="https://i.imgur.com/l1BLu6T.gif" width="500"/><br>
* Once this is done you should be good to go. Here is a list of all the items you must check:
  * Add an ``ansible`` user across all your Hosts
  * Generate and share an ssh key from your Control host to your other Hosts
  * Add the ``ansible`` user to the ``sudoers`` file for all the other Hosts

## Ansible Ad-hoc commands

**Ansible ad-hoc** commands are comparable to bash commands.
**Ansible playbooks** are analogous to a bash script.
For more information on Ansible ad-hoc commands visit the **[site](https://docs.ansible.com/ansible/latest/user_guide/intro_adhoc.html)**.

The basic syntax of an ad-hoc command is:<br>
``ansible [pattern] -m [module] -a "[module options]"``<br>
or<br>
``ansible [host] -b -m [module] -a "[arg1 arg2 argn]"``<br>

* ``host`` is a hot or host group defined in the Ansible inventory file
* ``-b`` is for become sudo
* ``-m`` is for module and allows a command to be used on a module
* ``-a`` allows for parameters' to be passed to the module

Examples:
* ``ansible anshost1 -m setup``
* ``ansible anshost1 -m ping``
* ``ansible anshost1 -b -m ansible.builtin.copy -a "src=/home/ans/ans-test.txt dest=/home/ans/"``
* ``ansible anshost1 -b -m service -a "name=apache2 state=started"``
* ``ansible anshost1 -b -m apt -a "name=apache2 state=latest"``

## Ansible Playbooks

Ansible playbooks are like bash scripts for Ansible. They are written in YAML and contain different elements called plays. Plays contain lists of hosts and tasks. Each task has a name and module. Modules have parameters. Spaces in those yaml files matter. Improper indentation can cause a playbook to err in a vague way.  

Here is an example of an ansible playbook command:
``ansible-playbook -i inv web.yml``<br>
This cinnabd wukk exectute the ``web.yml playbook`` using the ``inv file`` for hosts inventory. Both of which are at the present location.

Here is an example of how the web.yml playbook looks like:
```
---
- hosts: webservers
  become: yes

  tasks:
  - name: latest apache2 installed
    apt
      name: apache2
      state: latest
```
Here is a braeakdown of what this playbook will do:
1. The ``---`` lines mark this file as a YAML file
2. It will look for all the hosts that have been marked as webservers in the **inv** file we specified in the command
3. It will become **root**
4. It will try to find the **apache2** program
5. It will make sure that **apache2** is running on **latest** build

Here is an example of an **inv** file. We do not have to have such a file. We can also list all our hosts in the /etc/ansible/hosts file. But you can also have a seperate file like in our case called **inv** which in our case lives in /home/ans/inv

```
[webservers]
anshost1 ansible_host=192.168.56.3
anshost2 ansible_host=192.168.56.4
```
This basically tagges all the hosts under the [webservers] as webservers. And we can reference them in our playbooks as such.

Here is another example of a playbook yaml file.
```
---
- hosts: webservers
  become: yes

  tasks:
  - name: latest apache2 installed
    apt
      name: apache2
      state: latest
  - name: create index.html file
    file:
      name: /var/www/html/index.html
      state: touch
  - name: add web content
    lineinfile:
      line: "here is some text"
      path: /var/www/html/index.html
  - name: start apache2
    service:
    name: apache2
    state: started
```
Here is a braeakdown of what this playbook will do:
1. The ``---`` lines mark this file as a YAML file
2. It will look for all the hosts that have been marked as webservers in the **inv** file we specified in the command
3. It will become **root**
4. It will try to find the **apache2** program
5. It will make sure that **apache2** is running on **latest** build
6. It will create the **index.html** file in **/var/www/html/index.html**
7. It will add the **"here is some text"** line into the end of the **index.html**
8. It will make sure that the **apache2 service** is running

Each separate module has its own required parameters. Best thing would be to look for the parameters in the Ansible documentation.

Breaking down an ansible playbook command:<br>
``ansible-playbook -i inventory_file playbook.yml``<br>

## Ansible Variables

Variable can be scoped by group, host, or withing a playbook. They may be passed in via command line using the ``--extra-vars``, the ``-e`` flag, or defined within a playbook.

CLI Example:
``ansible-playbook service.yml -e "target_hosts=localhost target_service=http"``

Playbook Example:
```
---
hosts: webservers
become: yes
vars:
  target_service: http
  target_state: started
tasks:
  - name: Ensure target state
    service:
      name: "{{ target_service }}"
      state: "{{ target_state}}"
```

Varibles are referenced using double curly braces. It is a good practice to wrap variable names or statements containing variable names in weak quotes:

``hosts: "{{ my_host_variable }}"``

## Ansible Facts

Ansible facts are various properties regarding a given remote system that Ansible is going to collect for us.

You can use the **Setup** module to retrieve facts from a given host. <br>
``ansible anshost1 -m setup``<br>

The filter parameter takes regex to allow you to prune fact output. <br>
``ansible anshost1 -m setup -a filter=*ipv4*``<br>

Facts are gathered by default in the Ansible Playbook execution. Those facts can also be referenced as varibles in the playbook. The keyword ``gather_facts`` may be set in the playbook to change the fact gathering behavior.

The ansible command output may be directed to a file using the ``--tree outputfile`` flag which may be helpful when working with facts.

## Troubleshooting and Debugging

There are a couple of Ansible utilities that help us troubleshoot and debug

* **The debug module** is used to print detailed information about in-progress play. It is very handy for troubleshooting. The ``debug`` module takes two primary parameters that are mutually exclusive:
 * ``msg`` A message that is printed to STDOUT
 * ``var`` A variable whose content is printed to STDOUT

```
- debug:
    msg: "System {{ inventory_hostname }}" has uuid {{ ansible_product_uuid}}
```

* **The registered keyword** is used to store task output. Several attributes are available: ``return code``, ``stderr``, and ``stdout``

The following play will store the results of the shell module in a variable named motd_contents:

```
- hosts: all
  tasks:
	- shell: cat/etc/motd
	  register: motd_contents
```

## Ansible Handlers

Ansible provides a mechanism that allows an action to be flagged for execution when a task performs a change. By only executing certain tasks during a change, plays are more efficient. This mechanism is called a **handler**.

A handler may be called using the ``notify`` ketword:

```
- name: tempalte config file
  template:
	  src: foo.conf
		dest: /etc/foo.conf
  notify:
	  - restart memcached
```

No matter how many times a handler is flagged in a play, it only runs during the play's final phase. ``notify`` will only flag a handler if a task block makes changes. A handler may be defined similarly to tasks:

```
- name: restart memcached
  service:
	  name: memcached
		state: restarted
	listen: "restart cached services"

```
