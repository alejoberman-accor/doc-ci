# 1 Jobs

# 2 Administration

## 2.1 Upgrade Jenkins

Clone the repository :

https://github.com/AccorECOM/jenkins_docker.git

Edit `dockerfiles/jenkins.dockerfile` :

```bash
FROM jenkins/jenkins:2.193-jdk11
...
```

Increment the image number according to Jenkins repository. Commit and push.

Go to http://jenkins.ci-accor.io/job/Container%20Factory/. Launch the job.

Connect to jenkins.ci-accor.io :

```bash
cd CI/stack_cicd
#unlock the keychain with the password on https://vault.ci-accor.io
unlock-keychain -p ******
docker login appsregistryaccor.azurecr.io
docker pull appsregistryaccor.azurecr.io/jenkins
docker-compose -f jenkins.yml down -v
docker-compose -f jenkins.yml up -d
```
