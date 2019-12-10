# KPI-CLI

KPI-CLI is a command-line utility that extracts data exposed by the jenkins API and stores it into an Influxdb database. It parses the Jenkins json and transform it into InfluxDB json format.  

It is used by all Jenkins pipelines and consumes its plugin's API.

# Usage

You have to login to the Azure registry before.

```bash
docker login appsregistryaccor.azurecr.io/kpi-cli [show|write] kpi [args]
```

Credentials are stored on Azure portal (https://portal.azure.com).

## Commands

Two actions are possible : show and write. The command `show` extracts and print the kpi, the command `write` extracts and write the kpi in InfluxDB.

In most cases, Jenkins plugins have their own API. So for each plugin, a different command will be used to extract the data.

For debug only, the SHOW command lets you see the generated json in InfluxDB format :

```bash
docker login appsregistryaccor.azurecr.io show

NAME
    kpi-cli show - Display all jenkins stuff

SYNOPSIS
    kpi-cli show COMMAND

DESCRIPTION
    Display all jenkins stuff

COMMANDS
    COMMAND is one of the following:

     appsize
       Print app size in byte

     build_result
       Print build result for a specified job

     builds
       Print build result url

     checkstyle
       Print checkstyle

     checkstyle_android
       Print Android checkstyle

     checkstyle_result
       Print checkstyle

     coverage
       Print code coverage

     coverage_and
       Print android code coverage

     cpd
       Print duplicate code warnings

     cpd_legacy
       Print duplicate code warnings (legacy jenkins)

     jobs
       Print all jobs of a project

     sloc
       Print number lines statistics

     unit_tests
       Print unit tests results
```

For detailed information on each command, run:

```bash
docker run appsregistryaccor.azurecr.io/kpi-cli show [command] --help
```

# Configuration

Rename the file `kpi-cli.cfg.dist` to `kpi-cli.cfg` and configure Jenkins and InfluDB credentials.

The docker image is already configured by Jenkins.

# CI/CD

This repository is build by this job :

https://jenkins.ci-accor.io/job/Container%20Factory/
