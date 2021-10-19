# How To Install Docker On RHEL-8 / CENTOS-8/ FEDORA-35 

## Prerequisites 
- 	RHEL/CENTOS/FEDORA 64-bit operating system
- 	A user account with sudo privileges
- 	Command-line/terminal (ctrl-alt-t or applications menu > accessories > terminal)

 

Docker is a containerization technology that allows you to quickly build, test and deploy applications as portable, self-sufficient containers that can run virtually anywhere.

Follow these Steps  

Adding docker-ce repository. 
    sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
    sudo dnf repolist -v 

Removing **runc** and **podman** packages 
    
    sudo dnf remove runc podman* -y

Installing  **docker-ce** packges

    sudo dnf install --nobest  docker-ce

Installing **containerd.io** packages 
    sudo dnf install https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.2.6-3.3.el7.x86_64.rpm

Stop and disable firewall setting. 

    sudo systemctl disable firewalld
    sudo systemctl stop firewalld 

Start and enable docker service 

    sudo systemctl enable --now docker

Checking docker service status 

    systemctl is-active docker
    systemctl is-enabled docker

Installing **docker-compose** packages.

	curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o docker-compose

Installing required-packages.

	sudo dnf install python3-pip
	pip3.6 install docker-compose --user



## Docker Command line Interface

	docker [option] [subcommand] [arguments]
	docker –help 

## Docker Image
A Docker image is made up of a series of filesystem layers representing instructions in the image’s Dockerfile that makes up an executable software application. An image is an immutable binary file including the application and all other dependencies such as libraries, binaries, and instructions necessary for running the application.

## Search Docker Image
To search for an image from the Docker Hub registry, use the search subcommand.
For example, to search for an Ubuntu image, you would type:
    
    docker search ubuntu 

## Download Docker Images
For example, to download the latest official build of the Ubuntu 18.04 image, you would use the following image pull command:

	docker pull ubuntu

## Image List
To list all downloaded images type:

	docker image ls

## Remove an Image
If for some reasons, you want to delete an image, you can do that with the image rm [image_name] subcommand:

	docker image rmi ubuntu

## Docker Container
An instance of an image is called a container. A container represents a runtime for a single application, process, or service.
It may not be the most appropriate comparison, but if you are a programmer, you can think of a Docker image as class and Docker container as an instance of a class.
We can start, stop, remove, and manage a container with the docker container subcommand.

Start a container

    	docker container run -it ubuntu /bin/bash

List running container 

    	docker container ls 
    	docker container ls -a 

Remove container

    	docker container rm container_id 

Remove stop container history.

    	docker container rm prune -f
