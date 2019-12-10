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

# 1.2 Credentials

All credentials are stored in Vault :

https://vault.ci-accor.io

# 1.3 Monitoring

System dashboards :

https://db.ci-accor.io/chronograf/sources/1/dashboards

Servers status :

https://db.ci-accor.io/chronograf/sources/1/hosts


# 2. Access

# 2.1 SSH

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

# 2.2 Services

Jenkins : http://jenkins.ci-accor.io

Grafana : https://dashboard.ci-accor.io

InfluxDB : https://db.ci-accor.io

Chronograf : https://db.ci-accor.io/chronograf/
