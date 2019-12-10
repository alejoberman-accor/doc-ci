# Docker

All services are deployed in Docker containers. There is no container orchestrator to keep the architecture simple. Only Docker-compose is used to manage the services.

Each Azure VM expose these ports and run its own reverse proxy (Traefik):

* 443
* 80
* 8080: Proxy admin

| Name | Service hosted
------ | ---------------
| app-ci-vm-01 | Android Builder slave
| app-ci-vm-02 | Jenkins Master, Vault
| app-ci-timeseriedb | InfluxDB, Grafana, Chronograf

## File system setup

* `/datadrive/docker`: Docker working directory
* `/datadrive/app/service_name/stack`: contains `docker-compose.yml` which describe the stack
* `/datadrive/app/service_name/volume`: volume persistant data

## Manage services

Use the alias `service`:

Show available services:

```bash
service ls
```

Show running services:

```bash
docker ps
```

Start a service:

```bash
service service_name start
```

It's a wrapper of the `docker-compose` command.

## 1. Network architecture

![Apps CI-Accor](/assets/ci-net.jpg "Basic network architecture")

* Servers lists:

On Accor network :

| Name | Private IP | DNS 
------ | ---------- | ---
| ios-slave-01 (mac Pro) | 10.21.10.249 (vpn: 10.2.0.2) | ios-slave-01.ci-accor.io

On Azure :


| Name | Private IP | DNS 
------ | ---------- | ---
| app-ci-vm-01 | 10.0.0.5 | android-slave-01.ci-accor.io
| app-ci-vm-02 | 10.0.0.6 | jenkins.ci-accor.io
| app-ci-timeseriedb | 10.0.0.4 | db.ci-accor.io
