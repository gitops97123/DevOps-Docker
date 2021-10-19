
### Step-1:  List the images.
	# docker images   
	or
	# docker image ls 
 
### Step-2:  Now, Start the ubuntu containers:
	
	# docker container run -itd ubuntu bash  

	student@gitOps:~$ docker container ls 
	CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS          PORTS     NAMES       
	6e5900f9ba43   ubuntu    "bash"    43 seconds ago   Up 42 seconds             stoic_easley
 
Step-2:  Now, Commit the container and create new container image.
	
 	student@gitOps:~$ docker container commit 6e5900f9ba43 servera
	sha256:6a746ab5415859c908baa53c58e408e46dcdb8c38f115459e2fa934990992d94
 
	student@gitOps:~$ docker image ls 
	REPOSITORY   TAG       IMAGE ID       CREATED          SIZE  
	servera      latest    6a746ab54158   9 seconds ago    72.8MB
Step 3. Before push the container you have to sign-in with hub.docker.com.
	
	student@gitOps:~$ docker login
	Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
	Username: darwikdev11
	Password: 
	WARNING! Your password will be stored unencrypted in /home/student/.docker/config.json.
	Configure a credential helper to remove this warning. See
	https://docs.docker.com/engine/reference/commandline/login/#credentials-store

	Login Succeeded
	student@gitOps:~$`


Step-5:  Now, Define the tag. And push the image to docker hub.

	student@gitOps:~$ docker image tag servera darwikdev11/servera
	student@gitOps:~$ docker image ls 
	REPOSITORY            TAG       IMAGE ID       CREATED          SIZE  
	darwikdev11/servera   latest    6a746ab54158   11 minutes ago   72.8MB
	servera               latest    6a746ab54158   11 minutes ago   72.8MB

	# docker image tag darwikdev11/ansi_server  darwikdev11/ansi_server 
	# docker image ls 
	# docker push darwikdev11/ansi_server
 
Step-6:  Now, pushing client machine.
	
	# docker push darwikdev11/ansi_client  

 Search the image in private registry.
 
Step.6: After the pushed image, 
	
	Login: https://hub.docker.com 

you can check your images in repository.

![push](https://github.com/gitops97123/DockerOps/blob/main/icons/push.PNG?raw=true)


NOTE: Follow these instructions if you don’t have an account. 

Step-3.0: If you don’t have any account, Now, signup today.
Now, go to website : http://hub.docker.com 

 
Step-3.1: If you an account, Now, sign-in.

![login](https://github.com/gitops97123/DockerOps/blob/main/icons/docker_login.PNG?raw=true)

Step-3.2: After sign-in, you can check your repositories in your account.

Step-3.3: Create your own repository.
 
Step-3.4: tap to create a repository.
 

Step-3.5: after the created repository, look like this : 
 

Step-3.6: For remove : go to settings, and delete the repository.
  
Step-4: Search a new image.
 
Step-4.1: now you can pull the image.

	# docker pull nginx
 
