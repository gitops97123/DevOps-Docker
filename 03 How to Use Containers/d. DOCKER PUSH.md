
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


Step-4:  Now, Define the tag. And push the image to docker hub.

	student@gitOps:~$ docker image tag servera darwikdev11/servera
	student@gitOps:~$ docker image ls 
	REPOSITORY            TAG       IMAGE ID       CREATED          SIZE  
	darwikdev11/servera   latest    6a746ab54158   11 minutes ago   72.8MB
	servera               latest    6a746ab54158   11 minutes ago   72.8MB


 
Step-5:  Now, pushing client machine.
	
	student@gitOps:~$ docker push darwikdev11/servera  

 Search the image in private registry.
 
Step.6: After the pushed image, checkout the website  
	
	Login: https://hub.docker.com 

NOTE: Follow these instructions if you don’t have an account. 

Step-7.0: If you don’t have any account, Now, signup today.
Now, go to website : http://hub.docker.com 

![signup](https://github.com/gitops97123/DockerOps/blob/main/icons/signup.PNG?raw=true)


Step-7.1: If you an account, Now, sign-in.

![login](https://github.com/gitops97123/DockerOps/blob/main/icons/docker_login.PNG?raw=true)

Step-7.2: After sign-in, you can check your repositories in your account.

![push](https://github.com/gitops97123/DockerOps/blob/main/icons/push.PNG?raw=true)
 

Step-7.3: Create your own repository.

 ![crepo](https://github.com/gitops97123/DockerOps/blob/main/icons/crepo.PNG?raw=true)


Step-7.4: tap to create a repository.

![crepo1](https://github.com/gitops97123/DockerOps/blob/main/icons/crepo1.PNG?raw=true)


Step-7.5: after the created repository, look like this : 

 ![crepo2](https://github.com/gitops97123/DockerOps/blob/main/icons/crepo2.PNG?raw=true)


Step-7.6: For remove : go to settings, and delete the repository.

 ![drepo](https://github.com/gitops97123/DockerOps/blob/main/icons/drepo.PNG?raw=true)
 ![drepo1](https://github.com/gitops97123/DockerOps/blob/main/icons/drepo1.PNG?raw=true)

Step-8: Search a new image.

 ![srepo](https://github.com/gitops97123/DockerOps/blob/main/icons/srepo.PNG?raw=true)

Step-8.1: now go back to the termincal and pull the image.

	# docker pull mysql
 
