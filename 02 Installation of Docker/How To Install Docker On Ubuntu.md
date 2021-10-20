
# How To Install Docker On Ubuntu 20.04 / Linux Mint / Debian 

## Prerequisites 

- Ubuntu 18.04 64-bit operating system
- A user account with sudo privileges
- Command-line/terminal (CTRL-ALT-T or Applications menu > Accessories > Terminal)
- Docker software repositories (optional)

## Follow these Steps

## Installing pre-requisites packages 

    sudo apt update && sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

## Adding GPG-KEY

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

## Adding docker-ce repository
 
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

## Updating Packages

	sudo apt update

## Installing docker-ce packages 

	sudo apt install docker-ce

## Mark as hold the docker version 

	sudo apt-mark hold docker-ce

## Checking docker service status 

	sudo systemctl status docker

## Adding docker group in User account 

	sudo usermod -aG docker $USER


##  Upgrading Docker

When a new Docker version is released you can update the package using the standard upgrade process:

	sudo apt update

	sudo apt upgrade

## Uninstalling Docker: -

	sudo apt purge docker-ce

	sudo apt autoremove



## Docker Command line Interface: -

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

	docker image pull ubuntu

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

## Start a container

    	docker container run -it ubuntu /bin/bash

## List running container 

    	docker container ls 
    	docker container ls -a 

## Remove container

    	docker container rm container_id 

## Remove stop container history.

    	docker container rm prune -f
