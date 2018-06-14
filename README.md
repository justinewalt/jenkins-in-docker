# jenkins-in-docker

This repository contains _Jenkins_ master and agent `Dockerfiles`, [read how to use them](http://antonfisher.com/posts/2017/01/16/run-jenkins-in-docker-container-with-persistent-configuration/).

![Jenkins + Docker + gitHub](https://raw.githubusercontent.com/antonfisher/antonfisher.github.io/master/images/posts/8-run-jenkins-in-docker-container-with-persistent-configuration/run-jenkins-in-docker-container-with-persistent-configuration-logo.png)

## Clone the Repo
```sh
$ git clone https://github.com/MrJnrman/jenkins-in-docker.git
$ cd jenkins-in-docker
$ cd keys
```

Generate RAS keys for Jenkins master image. The first file is jenkins.config.id_rsa, this one will be used to access to Jenkins configuration on GitHub:
```sh
$ ssh-keygen -t rsa -b 4096 -C "my@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/af/.ssh/id_rsa): jenkins.config.id_rsa
```
Generate a second file jenkins.application.id_rsa, this one will be used to pull you application code Jenkins master container from GitHub.
```sh
$ ssh-keygen -t rsa -b 4096 -C "my@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/af/.ssh/id_rsa): jenkins.application.id_rsa
```

## Build The Docker Image
```sh
$ cd images/master
$ bin/image-build.sh
```

```sh
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
jenkins-master      latest              72f06a50ff88        5 hours ago         1.09GB
```

## Run Jenkins
```sh
$ cd images/master
$ bin/container-run.sh
```

At this point, Jenkins and the automation suite has been installed within the container.

Open up http://localhost:8080 in the browser.
Credentials:
``` sh
username: root
password: password123
```

Access the docker shell with the following command:
```sh
$ docker exec -it jenkins-master bash
```

In the docker shell run the following commands to execute the test suite:
```sh
root@a306ae5d9067:/# cd root/automation-suite
root@a306ae5d9067:/# npm test
```

Useful commands:

To exit the docker shell:
```sh
root@a306ae5d9067:/# exit
```

Stop the contianer:
```sh
$ ./bin/contianer-stop.sh
```

Build the docker image to get the most up to date version.
```sh
$ ./bin/image-build.sh
```

Remove the docker image.
```sh
$ ./bin/image-remove.sh 
```