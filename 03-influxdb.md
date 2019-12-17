# InfluxDB

InfluxDB is an open-source time series database (TSDB) developed by InfluxData. It is written in Go and optimized for fast, high-availability storage and retrieval of time series data in fields such as operations monitoring, application metrics, etc.

Accor CI/CD use it to store DevOPS KPI measurements like code coverage, checkstyle warnings, etc and system metrics for system monitoring.

## 1. Architecture

![Apps CI-Accor](/assets/tick.jpg "CI/CD data architecture")

Telgraf is an agent, it collects system metrics like memory usage, and send it to InfluxDB for storing them.

Grafana is the GUI for analysis and datav visualization.

[KPI-CLI](04-kpi-cli.md) is an ETL, it collects data on Jenkins API and application performance, in order to
monitor the application KPI.

### 1.2 Services

The different services are hosted on `db.ci-accor.io` and the docker-compose file path is `/datadrive/app/influxdb/stack/docker-compose.yml`

#### 1.2.1 InfluxDB

To push data intInfluxDB use the TCP/8086 port on `db.ci-accor.io`. 

Databases :

```bash
> show databases
name: databases
name
----
_internal
test
telegraf
kpi
```

##### 1.2.1.1 KPI database

Database `kpi` is used to store application KPI. Data are extracted by KPI-CLI ETL. A measurement is like a SQL table with a timestamp field.

```sql
> show measurements on kpi
name: measurements
name
----
app_size
build_result
checkstyle
cobertura_data
coverage
duplicate
jenkins_data
sloc
unit_test
```

`kpi` database is used to store application KPI and data are extracted by the KPI-CLI ETL.

##### 1.2.1.2 Telegraf database

`telegraf` database is powered by the different Telegraf agent running on every node. It contains the system and application lmetrics 

```bash
> show measurements on telegraf
name: measurements
name
----
cpu
disk
diskio
docker
docker_container_blkio
docker_container_cpu
docker_container_mem
docker_container_net
docker_container_status
influxdb
influxdb_cq
influxdb_database
influxdb_httpd
influxdb_memstats
influxdb_queryExecutor
influxdb_runtime
influxdb_shard
influxdb_subscriber
influxdb_tsm1_cache
influxdb_tsm1_engine
influxdb_tsm1_filestore
influxdb_tsm1_wal
influxdb_write
jenkins_job
jenkins_node
kernel
mem
net
netstat
ping
processes
swap
syslog
system
```

#### 1.2.2 Chronograf

Chronograf is the InfluxData’s open source web application. Chronograf is used with the other components of the TICK (Telegraf, InfluxDB, Chronograf, Kapacitor) stack to visualize monitoring data and create alerting and automation rules :

https://db.ci-accor.io/chronograf

Accor CI/CD use it to monitor the differents virtual machines and their services.

#### 1.2.3 Grafana

#### 1.2.4 Telegraf

Telegraf is an agent which runs on every CI/CD machine. It collects metrics about application and system to monitor the services.

# 2. Networking

Traefik is used as a reverse proxy in front of Docker engine :

![Apps CI-Accor](/assets/data_net.jpg "CI/CD data architecture")

## 3. Administration

On `db.ci-accor.io` as `accor-admin` user.

### 3.1 InfluxDB



### 3.1.1 InfluxDB CLI

```bash
docker-sh influxdb
influx
```

#### 3.1.2 Restart TICK stack

```bash
service influxdb restart
```

#### 3.1.3 Update Influxdb

```bash
service influxdb pull
service influxdb restart
```
