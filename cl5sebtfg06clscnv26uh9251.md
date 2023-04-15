---
title: "Docker and Docker Architecture"
datePublished: Tue Jul 19 2022 16:34:36 GMT+0000 (Coordinated Universal Time)
cuid: cl5sebtfg06clscnv26uh9251
slug: docker-and-docker-architecture
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1658248248687/TVXL14wGO.jpeg
tags: docker, beginner, devops

---

**What is Docker?**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/sdrzci16ps5z8bl7f5ax.png)
 
Docker is a tool designed to make it easier to build, create, deploy, run applications by using containers  and the use of containers to deploy applications is called **Containerization**.  

**What is a Container?**

A container is a standard unit of software that encapsulates all dependencies in order to make them easily  flexible, lightweight, secure, portable, deployable, updatable and also scale up or down. Furthermore  containers are one of the fastest means of deploying applications and services at scale on any supporting  hardware. Containers allow a developer to package an application with all the parts it needs, such as  libraries, dependencies, binaries and ship it out as one package. 

**Container vs Virtual Machine**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/biyhwfhxq8mxezt7dxnt.jpeg)
 
Virtual machines and containers are examples of virtualized environments, but they differ in their features  and size. Virtual machines are the traditional virtualization method and usually allow a computer to run a  few large VMs. 
Containers, by contrast, are a popular method for deploying microservices with more containers fitting onto  a single host. Whereas VMs use a hypervisor to create virtual instances, containers use a container engine  like Docker. 

**Why do we use docker?** 

Docker containers are lightweight, easy to create and deploy. Docker provides us with containers and also  helps solve the dependency problem by keeping dependency contained inside the containers. Basically,  the container solves the “it works on my machine” syndrome.

**Docker Architecture**

Before learning the Docker architecture, first, you should know about the **Docker Daemon** 

**What is Docker Daemon?** 

Docker daemon runs on a host machine and essentially acts as the brain of Docker. It creates and manages  your Docker images, containers, networking, and storage on your behalf. Its whole purpose is to perform  the commands that the client issues. It is responsible for running containers to manage docker services. 

**Docker architecture?**
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/iaut29y919mjwlyutosl.png)

Docker has a docker engine, which is a core part of the Docker system. It’s a client-server application and  it has three main components: 
- SERVER: it’s a long running process called **DAEMON PROCESS**. 
- A client which is Docker CLI (Command Line Interface) used to enter docker commands. 
- Rest API: It is used to instruct docker daemon on what to do i.e communicate between the client  and the server 
Docker client use commands and REST APIs to communicate with the Docker Daemon (Server). When a  client runs any docker command on the docker client terminal, the client terminal sends these docker  commands to the Docker daemon. Docker daemon receive these commands from the docker client in the  form of command and REST API's request. 

**Docker Registry?** 

Docker Registry manages and stores the Docker images and there are two types of registries in Docker  (public and private registry). 
- Public Registry - Docker-hub. 
- Private Registry - ECR(AWS), ACR(AZURE),GCR(GOOGLE) 

**Docker Object?**

When you are working with Docker, you use images, containers, volumes, networks; all these are Docker  objects.

**Docker Images?** 

Docker images are read-only blueprints with instructions to create a docker container. Images are  uploaded or pulled to Docker hub and also docker images are being created using a **Dockerfile**. 

**Docker Container?**

A container is the running instance of the image, All the applications and their environment run inside  this container. Containers can be start, stop, delete via Docker API or CLI 

**Docker Volumes** 

Volumes in docker allows you to persist data after a container dies 
- Normal volumes 
- Bind volumes 
- Anonymous volumes 

**Docker Network**

Docker network are ways through which all isolated containers communicate 
- Bridge Network 
- Host Network 
- Overlay Network 
- None Network 

**Docker Compose**

Multi-container tool to ease administration of 
- Images 
- Containers 
- Volumes 
- Network