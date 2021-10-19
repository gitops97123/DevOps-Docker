
### Step-1:  A fresh Ubuntu image from docker hub library.

 	student@gitOps:~$ docker pull ubuntu 
	Using default tag: latest
	latest: Pulling from library/ubuntu
	7b1a6ab2e44d: Pull complete
	Digest: sha256:626ffe58f6e7566e00254b638eb7e0f3b11d4da9675088f4781a50ae288f3322
	Status: Downloaded newer image for ubuntu:latest
	docker.io/library/ubuntu:latest

### Step-2:  Start a container.

	
	student@gitOps:~$ docker container run -it ubuntu /bin/bash
	root@734a364233ae:/#



### Step-3: List a container status.

	student@gitOps:~$ docker container ls -a
	CONTAINER ID   IMAGE     COMMAND       CREATED              STATUS                      PORTS     NAMES        
	734a364233ae   ubuntu    "/bin/bash"   About a minute ago   Exited (0) 19 seconds ago             sad_zhukovsky


### Step-4: Star last exit container.

	student@gitOps:~$ docker container start 734a364233ae
	734a364233ae

### Step-5: Also, you can use “exec” command to container shell.

	student@gitOps:~$ docker container exec -it 734a364233ae bash
	root@734a364233ae:/# exit
	exit

### Step-6: Also, you can use “attach” command to container shell.

 	student@gitOps:~$  docker container attach 734a364233ae 
	root@734a364233ae:/#

### Step-7: stop the running container.

 	student@gitOps:~$ docker container stop 734a364233ae  
	734a364233ae 


### Step-8: Also, you can use “kill” command to terminate container.

 	student@gitOps:~$ docker container kill 734a364233ae
	734a364233ae 
	

 ### Step-9: Start a container application named nginx in background.

	student@gitOps:~$ docker container run -itd nginx
	Unable to find image 'nginx:latest' locally
	latest: Pulling from library/nginx
	b380bbd43752: Pull complete
	fca7e12d1754: Pull complete
	745ab57616cb: Pull complete
	a4723e260b6f: Pull complete
	1c84ebdff681: Pull complete
	858292fd2e56: Pull complete
	Digest: sha256:644a70516a26004c97d0d85c7fe1d0c3a67ea8ab7ddf4aff193d9f301670cf36
	Status: Downloaded newer image for nginx:latest
	57534aec1c15ac8cd4de460a6da784a0b7b6034119145de2139334159998d578


### Step-10:  Use base machine Firefox to check out the website.

 	student@gitOps:~$ docker inspect 57534aec1c15 | grep  "IPAddress" | head -2 | tail -1 |  sed -e 's/,//g'
    "IPAddress": "172.17.0.2"
	student@gitOps:~$ curl http://172.17.0.2
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...

### Step-11:  Brought to the foreground container.

 	student@gitOps:~$ docker container wait 57534aec1c15 

### Step-12:  start a nginx web server with specific port.

 	student@gitOps:~$ docker container run -itd  -p 3838:80 nginx
	8e6db98dbfb33cd7fe2de33409147ea6fa645b9defa2e5bc9226000ccce3c0e3
	student@gitOps:~$

### Step-12:  Use remote machine Chrome to check out the website.

    student@gitOps:~$ curl  http://192.168.64.129:3838
	<!DOCTYPE html>
	<html>
	<head>
	<title>Welcome to nginx!</title>
	...
	</html>
 


