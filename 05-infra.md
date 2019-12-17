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

On each VM you can use the alias `service`:

Show available services:

```bash
# accor-admin @ app-ci-vm-01 in ~
$ service ls
android-slave  mock  traefik
```

Show running services:

```bash
docker ps
```

Start android slave service:

```bash
service android-slave start
```

It's a wrapper of the `docker-compose` command so all other commands are available.
