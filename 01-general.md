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
ssh accor-admin@jenkins.ci-accor.io
```

IOS Slave (this machine is in the internal network) : 

```bash
ssh accor-admin@ios-slave-01.ci-accor.io
```

Android slave :

```bash
ssh accor-admin@android-slave-01.ci-accor.io
```

Only machines from Sequana can connect on it.

# 2.2 Services

Jenkins : http://jenkins.ci-accor.io

Grafana : https://dashboard.ci-accor.io

InfluxDB : https://db.ci-accor.io

Chronograf : https://db.ci-accor.io/chronograf/
