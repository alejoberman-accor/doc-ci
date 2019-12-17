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

Go to http://portal.azure.com and download the VPN client and install it :

img_connected

### On Linux




s

4 import the base64 to rootcert to point à point 

5 Configure the client

1. import p12 file
2. 
