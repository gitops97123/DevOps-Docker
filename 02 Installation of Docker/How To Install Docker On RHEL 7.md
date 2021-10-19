# How To Install Docker On RHEL 7 /CENTOS 7 


**Prerequisites:**

- RHEL/CENTOS 64-bit operating system
- A user account with sudo privileges
- Command-line/terminal (ctrl-alt-t or applications menu > accessories > terminal)


Docker is a containerization technology that allows you to quickly build, test and deploy applications as portable, self-sufficient containers that can run virtually anywhere.

# Follow these Steps:  

Update the latest packages. 

    sudo yum update

Pre-requisites packages. 

	sudo yum install yum-utils device-mapper-persistent-data lvm2

Adding docker repo.

	sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

Installing "docker-ce" packages.

	sudo yum install docker-ce

Start and Enable docker services.

	sudo systemctl enable --now  docker

Checking service status. 

	sudo systemctl status docker

Checking docker status. 

	docker --version

Adding docker group to Local user 

    sudo usermod -aG docker $USER

## Docker Command line Interface: -
    
    docker [option] [subcommand] [arguments]
    
    docker --help 



## Search Docker Image

To search for an image from the Docker Hub registry, use the search subcommand.
For example, to search for an Ubuntu image, you would type:

    docker search ubuntu  

## Download Docker Image

A Docker image is made up of a series of filesystem layers representing instructions in the image’s Dockerfile that makes up an executable software application. An image is an immutable binary file including the application and all other dependencies such as libraries, binaries, and instructions necessary for running the application.

For example, to download the latest official build of the Ubuntu 18.04 image, you would use the following image pull command:

    docker pull ubuntu

## Image List
To list all downloaded images type:

	docker image ls

## Remove an Image
If for some reasons, you want to delete an image, you can do that with the image rm [image_name] subcommand:

	docker image rm ubuntu

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
