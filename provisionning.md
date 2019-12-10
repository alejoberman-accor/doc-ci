# Provisionning

Configuration management on Accor CI/CD plateform is managed by Ansible.

## Requirements

* Ansible installed
* VPN well configured
* Your SSH key have to be deployed

## Ansible

Ansible is an open-source software provisioning, configuration management, and application-deployment tool.

## Installation

On MacOS with pip

```bash
pip install ansible
```

On Linux

```bash
apt-get install ansible
```

```bash
yum install ansible
```

Deploy your SSH key and configure the VPN.

## Playbook

The plateform is quite simple and a single playbook is enough :

Since root Ansible repository, to configure all the VM on Azure, launch:

```bash
ansible-playbook -i inventory/priv-inventory playbook.yml
```
