## Setting command line
```
export PS1="\u@\h:\w$ "
```

## Install Docker
```
yum install -y docker
docker --version
docker --help
```

## Check docker status
```
service docker status
service docker start
service docker enable
```

## Install Git on linux server
```
yum install -y git
```

## OS Versions on Amz Linux
```
cat /etc/os-release 
```

## Search and pull a centos image
```
docker search centos
docker pull centos
```

## Docker run / exec commands:

### Start a container without entering its shell:
```
docker run -it -d <<container-id>> /bin/bash
```

### Start a container on a specific port
```
docker run -it -d -p <<host-port>>:<<docker-cont-port>> <<container-id>> /bin/bash
docker run -it -d -p 8081:8080 cd14cecfdb3a /bin/bash
docker run -it ubuntu:latest /bin/bash
```

## Graceful exit without terminating container
```
ctrl+p+q
```

## login to container later
```
docker exec -it <<c-id>> /bin/bash
```

## Check the details of a container
```
docker inspect <<c-id>>
```

## List docker images that are present on the system
```
docker images
```

## Start a container and provide it a specific name, docker run creates a new container from an existing image
```
docker run -ti -d <<c-id>> -name <<name-of-your-choice>>
docker run -ti -d 862a27a90ae5 -name vv-cont-apache /opt/entrypoint.sh
```

## List all running  / active docker containers
```
docker ps -a
```

## Connect to the Docker Cloud to push / pull containers

### login to the docker 
```
docker login
```

### pull a docker image from the docker cloud
```
docker pull <<repository-name>>/<<image-name>>:<<tag>>
docker pull vizagsol/mytestrepo_01:centos
```

### docker commit a running container which has changed files and settings to an image with new tag, it creates a new image
```
docker commit <<conatiner-id>> <<image-name:tag>>
docker commit 7194a324d3b1 vv-centos-docker:test01
```

### docker push into dockerhub repo

#### option 1
```
docker build -t vvcentos-docker-java:ans1 /opt/docker/java
docker tag vvcentos-docker-java:ans1 venkitram/vv-centos-repo:javatest01
docker push venkitram/vv-centos-repo:javatest01
```

#### option 2
```
docker build -t venkitram/vv-centos-repo:javatest02 /opt/docker/ansible/
docker push venkitram/vv-centos-repo:javatest02
```

#### option 2 - another example
```
docker build -t venkitram/centosday2:vvrepo1 /opt/docker/centos
docker push venkitram/centosday2:vvrepo1
```

## Create a Dockerfile. contents as below
```
FROM centos:latest
 
MAINTAINER vvdocker
 
WORKDIR /tmp
 
RUN yum install -y java-1.8.0-openjdk 
RUN useradd java
RUN mkdir -p /tmp/docker
RUN cd /tmp/docker
RUN touch f1 f2 f3
RUN echo "Hello Containers" _ /tmp/docker/f2
RUN 
 
WORKDIR /opt

USER java

CMD ["/bin/bash"]
```

## example docker file: 
> https://github.com/big-data-europe/docker-spark/blob/master/base/Dockerfile

## Another example of a Dockerfile - this installs java and copies entrypoint.sh from host to docker container
```
FROM centos:latest

WORKDIR /

MAINTAINER vvdocker

RUN useradd java
RUN mkdir -p /opt/myApp
RUN yum install -y java-1.8.0-openjdk

#COPY vvF1 /opt/myApp
#COPY vvF2 /opt/myApp
#COPY vvF3 /opt/myApp
#ADD vvDir1 /opt/myApp/

COPY entrypoint.sh /opt

ENTRYPOINT ["/opt/entrypoint.sh"]

USER java

CMD ["/bin/bash"]
```

## Docker **build** - Run the below command to provision the container with above Docker file
docker build -t vvcentos-docker-ansible:ans1 /opt/docker/ansible/
docker run -ti 69cf5edb658c

## Docker kill and remove images

### docker kill - stops a running and container and removes it
```
docker kill <<container-id>>
docker kill $(docker ps -q) 
```

### docker rmi - removes / deletes all the docker images
```
docker rmi -f $(docker images)
```

## exporting and import docker as containers
```
docker export <<container-id>> -o <<outpufile.tar>>
docker export b6ec8859012b -o vvcentos.tar
```
### copying the image over to another host and importing it there
The vvKey.txt will be the pem file that was used to login to the amazon linux instance
```
scp -i vvKey.txt /opt/docker/centos/vvcentos.tar ec2-user@172.31.16.49:/home/ec2-user
docker import /home/ec2-user/vvcentos.tar 
```

## Another way of moving as images is by performing the **save** and then the **load**
```
docker save _image.id_ file.tar
docker load < file.tar
```

## Dockerfile 
```
FROM centos:latest

MAINTAINER vvdocker

WORKDIR /
RUN mkdir -p /opt/myApp
COPY entrypoint.sh /opt/myApp/

RUN useradd apache && \
    mkdir -p /opt/myApp/dummy && \
    yum install httpd -y && \ 
    chmod -R 755 /opt/myApp

ENTRYPOINT ["/opt/myApp/entrypoint.sh"]
```

## entrypoint.sh that runs the apache process
```
rm -rf /run/httpd/* /tmp/httpd*
exec /usr/sbin/apachectl -DFOREGROUND
```

## Docker command to list the ip of a container
```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <<container-id>>
```

## Docker network commands
```
docker exec -ti 59ddc515f067 /bin/bash

docker network create vv-apache-network
docker inspect _network id_
docker network rm 6b68577b7175 9254ea53d9d2


docker network create -d bridge --subnet 10.25.0.0/16 vv-network
docker network inspect 2bae36943f5e
```
