
### Step-1:  A fresh Ubuntu image from docker hub library.

### Command:

 	# docker pull ubuntu 

### Status: 




### Step-2:  Start a container.
### Command:
	 # docker container run -it ubuntu /bin/bash 
### Status: 



### Step-3: List a container status.
### Command:
	# docker container ls 
    # docker container ls -a 

### Status: 
 


### Step-4: Star a stop container.
### Command:

 	# docker container start container_id

### Status: 
 


### Step-5: Also, you can use “exec” command to container shell.
### Command:

 	# docker container exec -it container_id bash  

### Status: 


### Step-6: stop the running container.
### Command:

 	# docker container stop container_id  
### Status: 



### Step-7: Also, you can use “exec” command to container shell.
### Command:

 	# docker container exec -it container_id bash  

### Status: 


### Step-8: Also, you can use “kill” command to terminate container.
### Command:

 	# docker container kill container_id   

### Status: 
 


 ### Step-9: Start a container application named nginx in background.
### Command:
 	# docker container run -itd nginx   
### Status: 


### Step-10:  Use base machine Firefox to check out the website.
### Command:

 	# firefox http://172.17.0.2 
### Status: 


### Step-11:  Brought to the foreground container.
### Command:

 	# docker container wait container_id 
### Status: 


### Step-12:  start a nginx web server with specific port.
### Command:

 	# docker container run -itd nginx -p 3838:80
### Status:  




### Step-12:  Use remote machine Chrome to check out the website.
### Command:

    curl  http://IP:3838

### Status: 
 


