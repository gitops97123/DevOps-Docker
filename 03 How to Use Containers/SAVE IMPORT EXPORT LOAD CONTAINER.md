
### Step-1:  Now, Commit the container and create new container image.

    student@gitOps:~$ docker container commit --author "DockerOps" b68db442a237 new_nginx:v1.0
    sha256:b0036d5b9633f8b28f5299aa966817d0c51e66b96fcb6b7728f49d09fda470f8
    
    student@gitOps:~$ docker image ls 
    REPOSITORY   TAG       IMAGE ID       CREATED         SIZE  
    new_nginx    v1.0      b0036d5b9633   7 seconds ago   133MB 

 
### Step-2:  Save the container image for backup purpose.

    student@gitOps:~$ docker image save new_nginx:v1.0 > new_nginx.tar 

### Step-3:  Remove the container image 

    student@gitOps:~$  docker image rm -f new_nginx:v1.0 
    Untagged: new_nginx:v1.0
    Deleted: sha256:b0036d5b9633f8b28f5299aa966817d0c51e66b96fcb6b7728f49d09fda470f8
    Deleted: sha256:dcffff0b57d2b3ba26532fe8c75f0d37b1b8c58f4ceea39c64fbfda0258f41dd

### Step-4:  Load the container.

    student@gitOps:~$ docker image load < new_nginx.tar   
    cf27dc43a80b: Loading layer [==================================================>]   12.8kB/12.8kB
    Loaded image: new_nginx:v1.0
    

