# Ansible Quick Start

## Ansible Introduction and Architecture

Ansible is an automation engine that allows for agentless system configuration and deployment.

Ansible operates **over SSH** and runs **Ansible Modules** on remote systems to complete tasks.

Ansible is typically **installed** on a single, lightweights **control node** where a list of host **inventory files** and **playbooks** are kept.

The heavy lifting is generally performed on the **remote host** as that is where **Ansible Modules** are executed.

<img src="https://miro.medium.com/max/512/0*Sr1T30hc8WT269jV" width="500"/><br>

## Ansible Configuration and Installation

For Ansible documentation check: https://docs.ansible.com/

In order to install Ansible you must configure the EPEL repository on your system. Once the EPEL repo is configured, your package manager installs Ansible and manages dependencies.

* On Red-Hat Systems:
``sudo yum install ansible``
and
``sudo yum install git``

* On Debian Systems:
``sudo apt update``
``sudo apt install software-properties-common``
``sudo add-apt-repository --yes --update ppa:ansible/ansible``
``sudo apt install ansible``

The primary Ansible configuration file is located in ``/etc/ansible/ansible.cfg``. Notable configurations there include:
 * ``Default inventory configuration``
 * ``Default remote user``

The Default Ansible Inventory File is ``/etc/ansible/hosts``. An inventory lis a list of hosts that Ansible manages.

The inventory location may be specified a couple of ways:
 * By Default: ``/etc/ansible/hosts``
 * Can be specified by CLI: ``ansible -i``
 * Can be set in ``/etc/ansible/ansible.cfg``
