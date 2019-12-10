# 1 Provisionning

Provisionning on Accor CI/CD plateform is managed by Ansible. 

# 2 Ansible

Ansible is an open-source software provisioning, configuration management, and application-deployment tool.[s its own declarative language to describe system configuration.

Ansible is agentless, temporarily connecting remotely via SSH or remote PowerShell to do its tasks.

# 3 Installation

On MacOS with Brew

```bash
brew install ansible
```

On Linux

```
apt-get install ansible
```

```
yum install ansible
```

# 4 Provisionning


Since root Ansible repository : 

```bash
ansible-playbook -i inventory playbook.yml
```

