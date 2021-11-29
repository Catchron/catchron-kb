Video Instructions: https://youtu.be/1Mr5sWkPXSk
Written Instructions: https://benprice.dev/posts/fvtt-docker-tutorial/

# Instructions:
### [1. Installing the required packages]()
### [2. Configuring and Running Traefik and Portainer Part 1]()
### [3. Configuring and Running Traefik and Portainer Part 2]()
### [4. Configuring and Running Traefik and Portainer Part 3]()


## 1. Installing the required packages

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
### Ansible Modules to use:<br>
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

  - name: Docker service is running
    ansible.builtin.systemd:
      name: docker
      enabled: yes
      state: started

  - name: Install all of the required packages
    apt:
      pkg:
      - apache2-utils
      - docker.io
      - docker-compose

  - name: Enable SSH, HTTP and HTTPS
    ufw:
      rule: allow
      port: 80,443,22
      proto: tcp

  - name: add the user to the docker group
    user:
      name: ans
      append: yes
      groups: docker

  - name: add a group for Docker
    ansible.builtin.group:
      name: docker




```
> Notes
``ansible anshost1 -b -m ufw -a "rule=allow port=80,443,222 proto=tcp``


## 2. Configuring and Running Traefik and Portainer Part 1

1. Generating a secure password. In the command below, substitute secure_password with the actual password that you want to use:<br>
``htpasswd -nb admin secure_password``<br>

2. The output will look something like this:<br>
``admin:$apr1$ruca84Hq$mbjdMZBAG.KWn7vfN/SNK/``<br>

Take note of the output, we will need it near the end of this step.<br>
> If your string has any $ you will need to modify them to be $$ - this is because docker-compose uses $ to signify a variable. By adding $$ we still docker-compose that it’s actually a $ in the string and not a variable.

3. Next up, we will start creating our config files. I recommend making a separate directory for each container. So, let’s make a folder and switch into it:<br>
``mkdir traefik && cd traefik``<br>

4. Now, let’s make our config file and start to edit it:<br>
``nano traefik.yml``<br>

5. Assuming you’re still where we left off, let’s go back up 1 level of our directory path, create a new directory for portainer, and switch into it.<br>
``cd .. && mkdir portainer && cd portainer``<br>

6. Portainer will create a fair number of files and directories that it needs to persist between runs. Let’s make a directory for all that.<br>
``mkdir data``<br>

7. Now, we’re going to create our docker-compose config file.<br>
``nano docker-compose.yaml``<br>

8. Copy/paste the contents below. We have 2 lines to modify after.<br>



-----------
### Ansible Modules to use:<br>
[htpasswd – manage user files for basic authentication](https://docs.ansible.com/ansible/2.9/modules/htpasswd_module.html#htpasswd-module)

```
- name: python-passlib
  apt:
    name: python3-passlib
    state: latest

- name: Generate a Password
  - htpasswd:
    path: /home/ans/anshtpasswd
    name: admin
    password: dmpass
    state: present
```
[ansible.builtin.shell – Execute shell commands on targets](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/shell_module.html)
```
- hosts: webservers
  become: yes
  tasks:
  - name: line in file and replace the $ symbol
    ansible.builtin.shell: sed 's/\$/\$$/g' /home/ans/anshtpasswd > /home/ans/anshtpasswd
```

``sed 's/\$/\$$/g' /home/ans/anshtpasswd > /home/ans/anshtpasswd``

[ansible.builtin.file – Manage files and file properties](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html)

```
- hosts: webservers
  become: yes
  tasks:
  - name: create the foundry folder
  become: yes
  tasks:
  - name: create the foundry folder
    ansible.builtin.file:
      path: /home/ans/traefik
      state: directory

[ansible.builtin.git – Deploy software (or files) from git checkouts](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/git_module.html)
```
```
- hosts: webservers
  become: yes
    tasks:
    - name: download the github repo for traefik and portainer
      ansible.builtin.git:
        repo: https://github.com/Catchron/dm-foundry-vtt.git
        dest: /home/ans/foundry-vtt
        force: yes
```

--------------

## 3. Configuring and Running Traefik and Portainer Part 2

1. Modify the email address from email@example.com to a valid email address in traefik.yml. This is used for generating your SSL certificates.

2. Replace Replace monitor.yourdomain.com with your own domain

3. Replace the auth information from the first command we ran

4. We’re nearly there. Now we need to create the docker network with a quick command:<br>
``docker network create proxy``

### Ansible Modules to use:<br>

[ansible.builtin.slurp – Slurps a file from remote nodes](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/slurp_module.html)

[ansible.builtin.replace – Replace all instances of a particular string in a file using a back-referenced regular expression](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/replace_module.html)

[docker_network – Manage Docker networks](https://docs.ansible.com/ansible/2.9/modules/docker_network_module.html#docker-network-module)


```
---
- hosts: webservers
  become: yes
  tasks:

  - name: add pass file to variable
    slurp:
      src: /etc/apache2/passwdfile
    register: pass_key

  - name: decode
    set_fact:
      remote_content: "{{pass_key.content | b64decode}}"

  - name: add email address
    ansible.builtin.replace:
      path: /home/ans/foundry-vtt/traefik/traefik.yml
      regexp: (example@domain.com)
      replace: admin@catchron.com

  - name: change domain
    ansible.builtin.replace:
      path: /home/ans/foundry-vtt/traefik/docker-compose.yml
      regexp: (yourdomain.com)
      replace: catchron.com

  - name: change keypassword
    ansible.builtin.replace:
      path: /home/ans/foundry-vtt/traefik/docker-compose.yml
      regexp: (keypassword)
      replace: "{{remote_content}}"

  - name: change domain in portainer
    ansible.builtin.replace:
    path: /home/ans/foundry-vtt/portainer/docker-compose.yml
    regexp: (yourdomain.com)
    replace: catchron.com

  - name: Create a network
    docker_network:
      name: proxy
```

--------------

## 4. Configuring and Running Traefik and Portainer Part 3

1. Run ``docker-compose up -d`` for traefik

2. Run ``docker-compose up -d`` for portainer

### 4. Ansible Modules to use:<br>

[docker_compose – Manage multi-container Docker applications with Docker Compose](https://docs.ansible.com/ansible/2.9/modules/docker_compose_module.html#docker-compose-module)

```
- name: docker compose for traefik
  docker_compose:
    project_src: /home/ans/foundry-vtt/traefik

- name: docker compose for portainer
  docker_compose:
    project_src: /home/ans/foundry-vtt/portainer

```
