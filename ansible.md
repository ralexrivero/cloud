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
