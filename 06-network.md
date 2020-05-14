# Networking

## 1. Network architecture

![Apps CI-Accor](/assets/ci-net.jpg "Basic network architecture")

* Servers lists:

On Accor network :

| Name | Private IP | DNS
------ | ---------- | ---
| ios-slave-01 (mac Pro) | 10.21.10.249 (vpn: 10.2.0.3) | ios-slave-01.ci-accor.io

On Azure :


| Name | Private IP | DNS
------ | ---------- | ---
| app-ci-vm-01 | 10.0.0.5 | android-slave-01.ci-accor.io
| app-ci-vm-02 | 10.0.0.6 | jenkins.ci-accor.io
| app-ci-timeseriedb | 10.0.0.4 | db.ci-accor.io

# VPN

This documentation describe how to setup a VPN point-to-point connection from an Azure network to a private network.

## Context

MacOS cannot run on virtual machine, you must have a physical machine if you want to build your application with XCode installed. It's impossible to run an iOS builder if your CI/CD infrastructure is hosted in the cloud. It was the case in my previous mission. One solution is to subscribe to a SaaS provider like Bitrise but if I choose this solution, I have to rewrite all the pipelines with their dedicated DSL. The team was familiar with Jenkins and all pipelines were already written. Moreover, the slaves are connected by SSH from the master to the slave. So it's impossible from the master to reach the mac inside the private company network.

Architecture :

* Virtual Network
* Virtual Network Gateway
* Mac Pro on the internal company network
* Jenkins on Azure in Azure network

## Setup the VPN Gateway

The main steps are :

1. Create Virtual Network
2. Create Subnet Gateway
3. Generate certificate

https://docs.microsoft.com/fr-fr/azure/vpn-gateway/vpn-gateway-certificates-point

This setup has already done and fully functionnal.

## Setup the VPN client

### On Windows

Go to http://portal.azure.com and download the VPN client and install the preconfigured package.

img_connected

### On Mac

#### Step 1 : generate certificate 

```bash
ssh accor-admin@android-slave-01.ci-accor.io
sudo su -
cd vpn
./create_vpn_access.sh yourSup3rp4ssword username
```

An archive contains the p12 certificat, fetch it on your machine, keep it safe and delete it after on `app-ci-vm-01`.

#### Step 2: import your p12 file

On your machine:

```bash
scp accor-admin@android-slave-01.ci-accor.io:/tmp/user_vpn.zip .
unzip user_vpn.zip
security import *.p12
```

#### Step 3: Configure your VPN

`System Preference > Network` 

Create a new VPN connection :

![Apps CI-Accor](/assets/vpn.png "VPN")

With these settings :

![Apps CI-Accor](/assets/vpn_new.png "VPN New")

##### Settings :

* Server address : `vpn.ci-accor.io`
* Remote ID : `azuregateway-0e3d0069-56f6-4640-beff-71132ecf22cf-cb1a548c9fc6.vpn.azure.com`
* Local ID : your username

Authentication settings : use your certificate previously installed.

Unlock the keychains.
