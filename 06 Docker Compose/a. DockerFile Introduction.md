
## Base Containers 

A Dockerfile is the mechanism that the docker packaging model provides to automate the building the container images. Building an image from a Dockerfile is a three-step process. 

1. Create a working directory. 
2. Write the Dockerfile specification 
3. Build the image with the docker command.


## Create a working directory. 

The Docker command can use the files in a working directory to build an image. An empty working directory should be created to keep from incorporating unnecessary files into the image. For security reasons, the root directory, /, should never be used a working directory for image builds.


Write the Dockerfile specification.

    # comment
	INSTRUCTION ARGUMENTS	

1. **FROM**  is the docker image.
2. **LABEL**   is responsible for adding generic metadata to an image.
3. **MAINTAINER** is responsible for setting the Author field of the generated container image. 
4. **RUN** execute commands in a new layer on top of the current image
5. **EXPOSE** indicates that the container listens on the specified network port at runtime.
6. **ENV** is responsible for defining environment variables that will be available to the container 
7. **ADD** copies files from local or remote source and add them to the container’s file system.
8. **COPY** copies files from local or remote source and add them to the container’s file system.
9. **USER** specifies the username 
10. **ENTRYPOINT** specifies the default command to execute when the container is created.
11. **CMD** provides the default arguments for the **ENTRYPOINT** instruction.

## Using CMD and ENTRYPOINT Instructions in the Dockerfile.

The ENTRYPOINT and CMD instructions have two formats: 
Using a JSON array:

**ENTRYPOINT [“command”, “param1”,” param2”]**

Using a shell form: 

**ENTRYPOINT command param1 param2**
**CMD param1 param2**

Example: 1 

 This repository contains Dockerfile of Nginx Container.

	# Pull base image.
	FROM darwikdev11/new-ubuntu
	# Install Nginx.
	RUN \
	  apt-get update && \
	  apt-get install -y nginx && \
	  rm -rf /var/lib/apt/lists/* && \
	  echo "\ndaemon off;" >> /etc/nginx/nginx.conf && \
	  chown -R www-data:www-data /var/lib/nginx
	
	# Define mountable directories.
	VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"]
	
	# Define working directory.
	WORKDIR /etc/nginx
	
	# Define default command.
	CMD ["nginx"]
	
	# Expose ports.
	EXPOSE 80
	EXPOSE 443
                                         

 when the Dockerfile is ready to run, execute the command:- 

	# docker build -t 01 . 
	# docker image ls 01
	root@devops-01:~/01# docker image ls 01
	REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
     	01           latest    5feafaf85934   6 minutes ago   337MB

     	#  docker container run -itd -p 82:80 01  
	root@devops-01:~/01# docker container run -itd -p 82:80  01
	9856f8ba3e91852e2dea8ea42148db0fba9f7bdb99f438efeaf9572dd1dcb355
	root@devops-01:~/01# docker container ls 
	CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS                                        NAMES
	9856f8ba3e91   01        "nginx"   4 seconds ago   Up 3 seconds   443/tcp, 0.0.0.0:82->80/tcp, :::82->80/tcp   busy_mayer
	root@devops-01:~/01# curl http://localhost:82 
	<!DOCTYPE html>
	<html>
	<head>
	<title>Welcome to nginx!</title>
	<style>
	    body {
	        width: 35em;
	        margin: 0 auto;
	        font-family: Tahoma, Verdana, Arial, sans-serif;
	    }
	</style>
	</head>
	<body>
	<h1>Welcome to nginx!</h1>
	<p>If you see this page, the nginx web server is successfully installed and
	working. Further configuration is required.</p>
	
	<p>For online documentation and support please refer to
	<a href="http://nginx.org/">nginx.org</a>.<br/>
	Commercial support is available at
	<a href="http://nginx.com/">nginx.com</a>.</p>
	
	<p><em>Thank you for using nginx.</em></p>
	</body>
	</html>

Example: 2 

	FROM ubuntu:18.04
	LABEL NAME "sam"
	RUN apt-get update &&  apt install -y python3-pip
	COPY test.py  /tmp
	COPY backup.tar /tmp
	WORKDIR /tmp
	ENTRYPOINT [ "python3", "./test.py" ]

when the Dockerfile is ready to run, execute the command:- 

 	root@devops-01:~/02# docker container run -it py 
	hello world

Example: 3 

    # this is a comment line
    FROM  centos
    LABEL description="This is a custom httpd container image"
    MAINTAINER sam anuragi <darwikdev@gmail.com>
    RUN yum install -y httpd
    EXPOSE 80
    ENV LogLevel "info"
    COPY index.html /var/www/html
    ENTRYPOINT ["/usr/sbin/httpd"]
    CMD ["-D","FOREGROUND"]
	
    root@devops-01:~/03# docker container run -itd  -p 82:80 apache:v2 
    73391f4ccc57e03b76c85a355b4489f206500e59e2d54fd95ee72532130b46c4
	
    root@devops-01:~/03# docker ps
    CONTAINER ID   IMAGE       COMMAND                  CREATED         STATUS         PORTS                               NAMES
    73391f4ccc57   apache:v2   "/usr/sbin/httpd -D …"   3 seconds ago   Up 2 seconds   0.0.0.0:82->80/tcp, :::82->80/tcp   goofy_mendel
	
    root@devops-01:~/03# curl http://localhost:82
    hello world

Example: 4 

    FROM ubuntu:18.04
    RUN apt-get update && apt-get install -y openssh-server \
       &&  mkdir /var/run/sshd && echo root:redhat | chpasswd \
       && sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
    # SSH login fix. Otherwise user is kicked off after login
    RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
    RUN useradd -c /bin/sh -d /home/sam -m sam && echo sam:redhat | chpasswd
    ENV NOTVISIBLE "in users profile"
    RUN echo "export VISIBLE=now" >> /etc/profile
    
    EXPOSE 22
    CMD ["/usr/sbin/sshd", "-D"]

