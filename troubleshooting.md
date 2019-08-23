This document give some tricks if an issue occurs on Accor CI.

# 1. Architecture

![Apps CI-Accor](/assets/ci-net.jpg "Basic network architecture")

# 1.1 Servers lists

On Accor network :

| Name | Private IP | DNS 
------ | ---------- | ---
| jenkins-master | 10.21.10.248 | jenkins.ci-accor.io
| ios-slave-01 (mac Pro) | 10.21.10.249 | ios-slave-01.ci-accor.io

On Azure :


| Name | Private IP | DNS 
------ | ---------- | ---
| app-ci-vm-01 | 10.0.0.5 | android-slave-01.ci-accor.io
| app-ci-timeseriedb | 10.0.0.4 | db.ci-accor.io

# 1.2 Monitoring

System dashboards :

https://db.ci-accor.io/chronograf/sources/1/dashboards

Servers status :

https://db.ci-accor.io/chronograf/sources/1/hosts

# 2. Access

# 2.1 Credentials

All credentials are stored in Vault :

https://vault.ci-accor.io

# 2.2 SSH

Jenkins master (this machine is on internal Accor network) : 

```bash
ssh cimobile@jenkins.ci-accor.io
```

IOS Slave (this machine is on internal Accor network) : 

```bash
ssh accor-admin@ios-slave-01.ci-accor.io
```

Android slave :

```bash
ssh accor-admin@android-slave-01.ci-accor.io
```

An IP restriction is set, only machines from Sequana can reach them.

# 2.1 Web

Jenkins : http://jenkins.ci-accor.io

Grafana : https://dashboard.ci-accor.io

InfluxDB : https://db.ci-accor.io

Chronograf : https://db.ci-accor.io/chronograf/

# 3. Issues

## 3.1 Jenkins Master is down or slow

This service is hosted by the Mac Mini.

Solutions :

1. Check if the Mac Mini is up
2. Check if there is no network outage
3. Check if there is no ip change

```bash
host jenkins.ci-accor.io
#MUST return 10.21.10.248
```

4. Check if Docker Desktop is launched
5. Check if Jenkins container is launched :

```bash
docker ps
```

If not, since your home directory, restart Jenkins :

```bash
cd CI/stack_cicd
docker-compose -f jenkins.yml down -v
docker-compose -f jenkins up -d
```

These commands will relaunch the entire stack.

## 3.2 No build on iOS

iOS build are made by the Mac Pro on internal network.

Solutions :

1. Check if the Mac Pro is up (sometimes there is sometimes power cut)
2. Check on Jenkins Master if the iOS slave is well connected :
    * On `Manage Jenkins` > `Manage Nodes`
    
![Apps CI-Accor](/assets/connected2.png "Basic network architecture")
  
## 3.3 No build on Androïd

The Androïd builder is hosted by a docker container on `android-slave-01.ci-acccor.io`. The Jenkins master is connected on it with dedicated port (SSH/9000). 

The integration tests need to
 access to REC environments on the Accor internal network. That's why a local Androïd builder is running on the Mac Pro.

If the container or/and the machine is down
Most of the time, the issue occurs because there is too much I/O operations during the build.

Solutions :

1. Check network connectivity

```bash
ping android-slave-01.ci-accor.io
```

2. Check SSH connection

```bash
ssh android-slave-01.ci-accor.io
```

If not, go on (you have to authenticate with Weboo on your smartphone to access the portal) :

https://portal.azure.com/

* Click on `All Ressources` > `app-ci-vm-01`

![Apps CI-Accor](/assets/azure-reboot.png "Restart VM")

* Click on restart and grab a coffee

1. Check if the container is up (you have to be root):

On `android-slave-01.ci-accor.io` :

```bash
sudo su -
docker ps
```

```
CONTAINER ID        IMAGE                                      COMMAND                  CREATED             STATUS              PORTS                                                              NAMES
53771418c269        appsregistryaccor.azurecr.io/android       "setup-sshd"             2 weeks ago         Up 20 hours         0.0.0.0:9000->22/tcp
```

2. Check on web jenkins interface is the agent is well connected.

![Apps CI-Accor](/assets/connected.png "Agent connected")