
### Step-1:  Start an existing container and perform some task in container.

    student@gitOps:~$ docker container start 7c0a418ab511
    7c0a418ab511  
 
### Step-2:  Check out the “diff” command.

    student@gitOps:~$ docker container diff 7c0a418ab511
    C /root
    A /root/.bash_history
    student@gitOps:~$  
 
### Step-3:  Checking Logs for the container. #DO NOT USE IMAGE NAME

    student@gitOps:~$ docker container logs 7c0a418ab511
    ...
    2022/03/19 07:32:00 [notice] 1#1: start worker processes
    2022/03/19 07:32:00 [notice] 1#1: start worker process 31
    ...

    student@gitOps:~$ 
 
### Step-3:  Checking stat for the container. #DO NOT USE IMAGE NAME

    student@gitOps:~$ docker container stats 7c0a418ab511
    CONTAINER ID   NAME             CPU %     MEM USAGE / LIMIT    MEM %     NET I/O       BLOCK I/O   PIDS
    7c0a418ab511   wizardly_curie   0.00%     1.504MiB / 3.81GiB   0.04%     3.42kB / 0B   0B / 0B     1   
    




