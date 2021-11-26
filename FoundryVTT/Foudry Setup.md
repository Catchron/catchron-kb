Video Instructions: https://youtu.be/1Mr5sWkPXSk
Written Instructions: https://benprice.dev/posts/fvtt-docker-tutorial/

# Instructions:

## Installing the required packages

1. Use the following command to install all of the required packages:<br>
``sudo apt update && sudo apt install apache2-utils docker.io docker-compose``<br>

2. Start docker and enable it to start on reboot:<br>
``sudo systemctl enable --now docker``<br>

3. Setup your firewall - we will only allow traffic to the server over SSH, HTTP and HTTPS:<br>
``sudo ufw allow 22``<br>
``sudo ufw allow 80``<br>
``sudo ufw allow 443``<br>
``sudo ufw enable``<br>

5. Last up, we need to give your user access to the docker group, with these last 2 commands.<br>
``sudo usermod -aG docker $USER``<br>
``newgrp docker``<br>

----------------
Ansible Modules to use:
[apt – Manages apt-packages](https://docs.ansible.com/ansible/2.9/modules/apt_module.html#apt-module)<br>
[ansible.builtin.systemd – Manage systemd units](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/systemd_module.html)<br>
[ufw – Manage firewall with UFW](https://docs.ansible.com/ansible/2.9/modules/ufw_module.html#ufw-module)<br>
[user – Manage user accounts](https://docs.ansible.com/ansible/2.9/modules/user_module.html#user-module)<br>
[ansible.builtin.group – Add or remove groups](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/group_module.html)<br>

1. **Playbook:** Install all of the required packages:<br>



```
---

- hosts: webservers
  become: yes
  tasks:
  - name: Update all packages to the latest version
    apt:
      upgrade: dist

  - name: Install all of the required packages
    apt:
      pkg:
      - apache2-utils
      - docker.io
      - docker-compose

  - name: Enable SSH, HTTP and HTTPS
    ufw:
      rule: allow
      port: 80,443,222
      proto: tcp
# ansible anshost1 -b -m ufw -a "rule=allow port=80,443,222 proto=tcp"

  - name: add a group for Docker
    ansible.builtin.group:
      name: docker

  - name: add the user to the docker group
    user:
      name: ans
      append: yes
      groups: docker

  - name: Docker service is running
    ansible.builtin.systemd:
      name: docker
      enabled: yes
```
