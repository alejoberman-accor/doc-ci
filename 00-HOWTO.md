# How to

## Connect to Jenkins

Deploy your SSH pubkey.

## Restart Jenkins

```bash
ssh accor-admin@jenkins.ci-accor.io
service jenkins restart
```

## Upgrade Jenkins

```bash
ssh accor-admin@jenkins.ci-accor.io
# Edit the Jenkins Dockerfile
# Increment the docker tag :
# FROM jenkins/jenkins:2.206-jdk
vim jenkins_docker/dockerfiles/jenkins.dockerfile
git commit -a -m "update Jenkins to 2.207-jdk"
git push
```

Go to :

https://jenkins.ci-accor.io/job/Container%20Factory/job/master/

Launch and build the Jenkins image.

```bash
service jenkins restart
```
