# Ansible

Ansible is a configuration management tool that allows you to automate the configuration of your servers. It is a simple tool that can be used to configure a single server or a large number of servers.

## Installation

Ansible is available in the Ubuntu repositories. To install it, run the following command:

- create non root user with sudo privileges
- install ansible
- `sudo apt-add-repository ppa:ansible/ansible`
- `sudo apt update`
- `sudo apt install ansible`

- Setup the inventory file

- `sudo nano /etc/ansible/hosts`
- replace the IPs of the servers

```bash
[servers]
server1 ansible_host=203.0.113.111
server2 ansible_host=203.0.113.112
server3 ansible_host=203.0.113.113

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```

- check the inventory

- `ansible-inventory --list -y`

- Test conection

- `ansible all -m ping -u root`

- Run ad-hoc commands

- `ansible all -a "free -m" -u root` - check memory
- `ansible all -a "df -h" -u root` to check disk usage
- `ansible all -a "uptime" -u root` to check uptime

- Execute Ansible modules

- `ansible all -m apt -a "name=nginx state=present" -u root` to install nginx
- `ansible servers -a "uptime" -u root` to check uptime on servers group
- `ansible server1:server2 -m ping -u root` specify multiple hosts

## Setup workspace and Ansible inventory file

- create a directory for the project

- `mkdir kube-cluster`
- `cd kube-cluster`

- crate the initial.yml file

- `vim initial.yml`

```yaml
---
- hosts: all
  become: yes
  tasks:
    - name: create the 'ubuntu' user
      user: name=ubuntu append=yes state=present createhome=yes shell=/bin/bash

    - name: allow 'ubuntu' to have passwordless sudo
      lineinfile:
        dest: /etc/sudoers
        line: 'ubuntu ALL=(ALL) NOPASSWD: ALL'
        validate: 'visudo -cf %s'

    - name: set up authorized keys for the ubuntu user
      authorized_key: user=ubuntu key="{{item}}"
      with_file:
        - ~/.ssh/id_rsa.pub
```

- run the playbook locally

- `ssh-add tify` to add the ssh key to the ssh-agent
- `ansible-playbook -i hosts initial.yml` to run the playbook and add ubuntu user

- Install kubernetes dependencies
  - `Docker`
  - `kubeadm`
  - `kubelet`
  - `kubeclt`

- `ansible-playbook -i hosts ~/code/kube-cluster/kube-dependencies.yml` installs kubeadm and kubectl

- `ansible-playbook -i hosts ~/code/kube-cluster/control-plane.yml` to install flannel

- ssh into the control plane
- `kubectl get nodes` to check the status of the nodes

- `sudo kubeadm init --pod-network-cidr=10.244.0.0/16`
