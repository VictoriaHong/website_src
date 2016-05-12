---
title: Docker
date: 2016-04-21 12:38:04
tags: [Docker, Notes]
categories:
- Tech Notes
layout: true
---

## Overview

**Docker Structure**

![](http://i.imgur.com/rVrlbf8.png)

<!--more--->

### Basic Concept

- is a platform for developing , shipping and running applications using container virtualization technology

- Traditional solution:

	```
	app --> OS --> physical sever
	```

	Problems: slow; cost; waste; scale; migrate; vendor lock in

- Hypervisor-based Virtualization

	```
	VM( app --> guest OS )

	VM( app --> guest OS ) --> Hypervisor --> host OS --> physical sever

	VM( app --> guest OS )
	```
	Benefits: better resource pooling; scale; VMs in cloud

	Limitations: VMs need resources

- Container

	Container based virtualization uses the kernel on the host's OS to run multiple **guest instances**(container)

	```
	container( app )

	container( app ) --> Host OS( OS Kernel ) --> Physicals server

	container( app )
	```
Container over VM: lightweight; less resource; portability
- Docker

	```
	container( app )

	container( app ) --> Docker Engine --> Linux OS( Linux Kernel ) --> Physicals server

	container( app )
	```

	**Namespace** is used to support isolated containers


- Docker Client and Daemon

	client --> Daemon(server)

	run on same host/diff hosts

- Docker Container and Image

	image

	- read only template to create containers
	- Docker hub or local registry

	Container

	- All resources needed to run the app

	Registry & Repository

	- **Docker Hub** is a registry
	- Registry contains several repos (repo for ubuntu, for redhat)
	- Repo contains several images

- Docker Orchestration

	Machine/Swarm/Compose

- Benefits: Separation; Fast deployment cycle; Portability; Scalability

### Operations

- Show images

	`docker image`

- Container ID

	long ID and short ID

	`docker ps` list containers

	`docker ps -a` list all containers

- Running options

	- Running in Detached Mode

		`docker run -d command`

	- Running a web app inside a container

		`docker run -d -P tomcat:7` map the tomcat ports to the host port randomly(49153 ~ 65535)

		`docker run -d -p 8080:80 nginx:1.7` map 80 on the container to 9090 on the host

	- attach stdin

		`docker run -i`

  	- pseudo-tty

  		`docker run -t`

- Commit
	`docker commit [options] [container ID] [repo: tag]`

	refer to a container by ID or name

	repo name: username + / + appname

	default tag: latest


### Building Image

- Image Layers

	- a layer is also just another image
	- layers are read only

- Writable Layer

	all changes are made to the writable layer

- Dockerfile

	is a configuration file that contains instructions for building a Docker image

	instructions can be cached automatically

	**FROM** base image

	`FROM ubuntu:14.04`

	**RUN** specify a command to execute

	each RUN is a new commit, so we can aggregate multiple RUN instructions

	`RUN apt-get update && apt-get install vim`

	**CMD** defines a default command to execute when a container is created

	can be specified once in a Dockerfile and can be overridden at runtime

	shell form: `CMD ping 127.0.0.1 -c 30`

	exec form: `CMD ["ping", "127.0.0.1", "-c", "30"]` can pass in parameters at runtime

	**ENTRYPOINT** can't be overridden at runtime

	`ENTRYPOINT ["ping"]`

	**VOLUME**

	`VOLUME /directory`

	`VOLUME /www/website1.com /www/website2.com` multiple volumes

	can't map volumes to host directories

	**EXPOSE** configure which port a container will listen on at runtime

	`EXPOSE 80 443`

- Docker Build

	`docker build [options] [path]` path is build context(including all resource files)

	`docker build -t [repo:tag] [path]`

- Start and Stop Containers

	`docker ps -a`

	`docker start <container ID or name>`

	`docker stop <container ID or name>`

- Getting terminal access

	`docker exec` to start another process within a container

	`docker exec -it [container ID] /bin/bash`

	`ps -ef` to see existing processes in this container

- Delete Container

	 `docker stop`
	 `docker rm [container ID]`

- Delete local images

	`docker rmi [image ID]`

	`docker rmi [repo:tag]`

- Push image to docker hub

	`docker push [repo:tag]`


### Volumes

- is a designated directory in a container, which is designed to persist data, independent of the container's life cycle

- persist when a container is deleted

- Mount a Volume

	`docker run -i -t -v /data/src:/test/src nginx:1.7`

	execute a new container and map the /data/src  folder from the host into the /test/src folder in the container


### Networking Basics

- **EXPOSE**

- Link Containers

	`docker run -d --name database postgres` create the source container first

	`docker run -d -p --name website -- link database:db nginx` create the recipient container

	db is alias for database


### Bittiger讲解

- union file system:

	上层会覆盖下层的文件。

- copy on write:

	需要改什么文件，就从下层拿什么文件上来。但所有的修改都隔离的，一旦container删了，东西就都丢了。可以用volume映射。

- Docker use namespace to achieve isolation

	避免资源竞争

- Docker use control group to achieve resource limiting

	linux kernel functionality

 - runC

 	an abstraction layer between Docker Driver anD Linux kernel

	it interfaces with Linux kernel functionalities
