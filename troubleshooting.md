This document give some tricks if an issue occurs on Accor CI.

# 1. Architecture

![Apps CI-Accor](/assets/ci-net.jpg "Basic network architecture")

# 1.1 Network topology



# 2. Access

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

There is an IP restriction, all of these machines can only be accessed from Sequana.

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

If not, since your home directory, you can copy paste on your terminal:

```bash
cd CI/stack_cicd
docker-compose -f jenkins.yml down -v
docker-compose -f jenkins up -d
```

These commands will relaunch the entire stack.

## 3.2 No build on iOS

The iOS build is made by the Mac Pro.

Solutions :

1. Check if the Mac Pro is up (yes sometimes there is some electricity outage)
2. Check on Jenkins Master if the iOS slave is well connected :
    * On Manage Jenkins > Node 
    * Check the logs
  

## 3.3 No build on Androïd

The Androïd builder is hosted by a docker container on `android-slave-01.ci-acccor.io`. The Jenkins master is connected on it with dedicated port (SSH/9000).

If the container or/and the machine is down : [^1] No ping ? No SSH ? Most of the time, the issue occurs because there is too much I/O operation and the charge is too high.

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

![Apps CI-Accor](/assets/azure-reboot.png "Basic network architecture")

* Click on restart and grab a coffee

1. Check if the container is up (you have to be root):

On `android-slave-01.ci-accori.io` :

```bash
sudo su -
docker ps
```

```
CONTAINER ID        IMAGE                                      COMMAND                  CREATED             STATUS              PORTS                                                              NAMES
53771418c269        appsregistryaccor.azurecr.io/android       "setup-sshd"             2 weeks ago         Up 20 hours         0.0.0.0:9000->22/tcp
```

2. Check on Jenkins master is the agent is well connected.