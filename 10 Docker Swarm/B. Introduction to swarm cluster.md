# Swarm mode overview
To use Docker in swarm mode, install Docker. See installation instructions for all operating systems and platforms.

Current versions of Docker include swarm mode for natively managing a cluster of Docker Engines called a swarm. Use the Docker CLI to create a swarm, deploy application services to a swarm, and manage swarm behavior.

Docker Swarm mode is built into the Docker Engine. Do not confuse Docker Swarm mode with Docker Classic Swarmopen_in_new which is no longer actively developed.


## How to Install and Configure Docker Swarm on Ubuntu

Docker Swarm is a clustering tool that turns a group of Docker hosts into a single virtual server. Docker Swarm ensures availability and high performance for your application by distributing it over the number of Docker hosts inside a cluster. Docker Swarm also allows you to increase the number of container instance for the same application. Clustering is an important feature of container technology for redundancy and high availability. You can manage and control clusters through a swarm manager. The swarm manager allows you to create a primary manager instance and multiple replica instances in case the primary instance fails. Docker Swarm exposes standard Docker API, meaning that any tool that you used to communicate with Docker (Docker CLI, Docker Compose, Krane, and Dokku) can work equally well with Docker Swarm. Features of Swarm Mode:

    Provides Docker Engine CLI to create a swarm of Docker Engines where you can deploy application services. You don't need any additional software tool to create or manage a swarm.
    Swarm manager assigns a unique DNS name to each service in the swarm. So you can easily query every container running in the swarm through a DNS name.
    Each node in the swarm enforces TLS mutual authentication and encryption to secure communications between itself and all other nodes. You can also use self-signed root certificates or certificates from a custom root CA.
    Allows you to apply service updates to nodes incrementally.
    Allows you to specify an overlay network for your services and how to distribute service containers between nodes.

In this post, we will go through how to install and configure Docker Swarm mode on an Ubuntu 16.04 server. We will:

    Install one of the service discovery tools and run the swarm container on all nodes.
    Install Docker and configure the swarm manager.
    Add all the nodes to the Manager node (more on nodes in the next section).

To get the most out of this post, you should have:

    Basic knowledge of Ubuntu and Docker.
    Two nodes with ubuntu 16.04 installed.
    A non-root user with sudo privileges setup on both nodes.
    A static IP address configured on Manager Node and on Worker Node. Here, we will use IP 192.168.0.103 for Manager Node and IP 192.168.0.104 for Worker Node.

Let's start by looking at nodes.

## Manager Nodes and Worker Nodes

Docker Swarm is made up of two main components:

    Manager Nodes
    Worker Nodes

Manager Nodes Manager nodes are used to handle cluster management tasks such as maintaining cluster state, scheduling services, and serving swarm mode HTTP API endpoints. One of the most important features of Docker in Swarm Mode is the manager quorum. The manager quorum stores information about the cluster, and the consistency of information is achieved through consensus via the Raft consensus algorithm. If any Manager node dies unexpectedly, other one can pick up the tasks and restore the services to a stable state. Raft tolerates up to (N-1)/2 failures and requires a majority or quorum of (N/2)+1 members to agree on values proposed to the cluster. This means that in a cluster of 5 Managers running Raft, if 3 nodes are unavailable, the system cannot process any more requests to schedule additional tasks. The existing tasks keep running but the scheduler cannot rebalance tasks to cope with failures if the manager set is not healthy. Worker Nodes Worker nodes are used to execute containers. Worker nodes don’t participate in the Raft distributed state and don't make scheduling decisions. You can create a swarm of one Manager node, but you cannot have a Worker node without at least one Manager node. You can also promote a worker node to be a Manager when you take a Manager node offline for maintenance. As mentioned in the introduction, we use two nodes in this post — one will act as a Manager node and other as a Worker node.
Getting Started

Before starting, you should update your system repository with the latest version. You can update it with the following command:

    sudo apt-get update -y && sudo apt-get upgrade -y Once the repository is updated, restart the system to apply all the updates.

## Install Docker

You will also need to install the Docker engine on both nodes. By default, Docker is not available in the Ubuntu 16.04 repository. So you will need to set up a Docker repository first. You can start by installing the required packages with the following command:

sudo apt-get install apt-transport-https software-properties-common ca-certificates -y Next, add the GPG key for Docker: wget https://download.docker.com/linux/ubuntu/gpg && sudo apt-key add gpg Then, add the Docker repository and update the package cache: sudo echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable" >> /etc/apt/sources.list sudo apt-get update -y Finally, install the Docker engine using the following command: sudo apt-get install docker-ce -y Once the Docker is installed, start the Docker service and enable it to start on boot time: sudo systemctl start docker && sudo systemctl enable docker By default, Docker daemon always runs as the root user and other users can only access it using sudo. If you want to run docker command without using sudo, then create a Unix group called docker and add users to it. You can do this by running the following command: sudo groupadd docker && sudo usermod -aG docker dockeruser Next, log out and log back to your system with dockeruser so that your group membership is re-evaluated. Note: Remember to run the above commands on both nodes.
Configure firewall

You will need to configure firewall rules for a swarm cluster to work properly on both nodes. Allow the ports 7946, 4789, 2376, 2376, 2377, and 80 using the UFW firewall with the following command:

sudo ufw allow 2376/tcp && sudo ufw allow 7946/udp && sudo ufw allow 7946/tcp && sudo ufw allow 80/tcp && sudo ufw allow 2377/tcp && sudo ufw allow 4789/udp Next, reload the UFW firewall and enable it to start on boot: sudo ufw reload && sudo ufw enable Restart the Docker service to affect the Docker rules: sudo systemctl restart docker
Create Docker Swarm cluster

First, you will need to initialize the cluster with the IP address, so your node acts as a Manager node. On the Manager Node, run the following command for advertising IP address:
    
    docker swarm init --advertise-addr 192.168.0.103 You should see the following output:

Swarm initialized: current node (iwjtf6u951g7rpx6ugkty3ksa) is now a manager.

To add a worker to this swarm, run the following command:
    docker swarm join --token SWMTKN-1-5p5f6p6tv1cmjzq9ntx3zmck9kpgt355qq0uaqoj2ple629dl4-5880qso8jio78djpx5mzbqcfu 192.168.0.103:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

The token shown in the above output will be used to add worker nodes to the cluster in next step. The Docker Engine joins the swarm depending on the join-token you provide to the docker swarm join command. The node only uses the token at join time. If you subsequently rotate the token, it doesn’t affect existing swarm nodes. Now, check the status of the Manager Node with the following command: docker node ls If everything is fine, you should see the following output:

    ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
    iwjtf6u951g7rpx6ugkty3ksa *   Manager-Node        Ready               Active              Leader

You can also check the status of the Docker Swarm Cluster: code>docker info

You should see the following output:
    
    Containers: 0
     Running: 0
     Paused: 0
     Stopped: 0
    Images: 0
    Server Version: 17.09.0-ce
    Storage Driver: overlay2
     Backing Filesystem: extfs
     Supports d_type: true
     Native Overlay Diff: true
    Logging Driver: json-file
    Cgroup Driver: cgroupfs
    Plugins:
     Volume: local
     Network: bridge host macvlan null overlay
     Log: awslogs fluentd gcplogs gelf journald json-file logentries splunk syslog
    Swarm: active
     NodeID: iwjtf6u951g7rpx6ugkty3ksa
     Is Manager: true
     ClusterID: fo24c1dvp7ent771rhrjhplnu
     Managers: 1
     Nodes: 1
     Orchestration:
      Task History Retention Limit: 5
     Raft:
      Snapshot Interval: 10000
      Number of Old Snapshots to Retain: 0
      Heartbeat Tick: 1
      Election Tick: 3
     Dispatcher:
      Heartbeat Period: 5 seconds
     CA Configuration:
      Expiry Duration: 3 months
      Force Rotate: 0
     Autolock Managers: false
     Root Rotation In Progress: false
     Node Address: 192.168.0.103
     Manager Addresses:
      192.168.0.103:2377
    Runtimes: runc
    Default Runtime: runc
    Init Binary: docker-init
    containerd version: 06b9cb35161009dcb7123345749fef02f7cea8e0
    runc version: 3f2f8b84a77f73d38244dd690525642a72156c64
    init version: 949e6fa
    Security Options:
     apparmor
     seccomp
      Profile: default
    Kernel Version: 4.4.0-45-generic
    Operating System: Ubuntu 16.04.1 LTS
    OSType: linux
    Architecture: x86_64
    CPUs: 1
    Total Memory: 992.5MiB
    Name: Manager-Node
    ID: R5H4:JL3F:OXVI:NLNY:76MV:5FJU:XMVM:SCJG:VIL5:ISG4:YSDZ:KUV4
    Docker Root Dir: /var/lib/docker
    Debug Mode (client): false
    Debug Mode (server): false
    Registry: https://index.docker.io/
    Experimental: false
    Insecure Registries:
     127.0.0.0/8
    Live Restore Enabled: false

## Add Worker Node to swarm cluster

Manager Node is now configured properly, it's time to add Worker Node to the Swarm Cluster. First, copy the output of the "swarm init" command from the previous step, then paste that output on the Worker Node to join the Swarm Cluster:

    docker swarm join --token SWMTKN-1-5p5f6p6tv1cmjzq9ntx3zmck9kpgt355qq0uaqoj2ple629dl4-5880qso8jio78djpx5mzbqcfu 192.168.0.103:2377

You should see the following output:

**This node joined a swarm as a worker.**

Now, on the Manager Node, run the following command to list the Worker Node: docker node ls

You should see the Worker Node in the following output:
    
    ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
    iwjtf6u951g7rpx6ugkty3ksa *   Manager-Node        Ready               Active              Leader
    snrfyhi8pcleagnbs08g6nnmp     Worker-Node         Ready               Active

## Launch web service in Docker Swarm

Docker Swarm Cluster is now up and running, it's time to launch the web service inside Docker Swarm Mode. On the Manager Node, run the following command to deploy a web server service: docker service create --name webserver -p 80:80 httpd

The above command will create an Apache web server container and map it to port 80, so you can access Apache web server from the remote system. Now, you can check the running service with the following command: docker service ls You should see the following output:

    ID                  NAME
    MODE                REPLICAS            IMAGE               PORTS
    nnt7i1lipo0h        webserver           replicated          0/1                 apache:latest       *:80->80/tcp

Next, scale the web server service across two containers with the following command: docker service scale webserver=2

Then, check the status of web server service with the following command: docker service ps webserver You should see the following output:

    ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE                  ERROR               PORTS
    7roily9zpjvq        webserver.1         httpd:latest        Worker-Node         Running             Preparing about a minute ago
    r7nzo325cu73        webserver.2         httpd:latest        Manager-Node        Running             Preparing 58 seconds ago

## Test Docker Swarm

Apache web server is now running on Manager Node. You can now access web server by pointing your web browser to the Manager Node IP

or Worker Node IP as shown below: Screenshot-of-docker-swarm-apache Apache web server service is now distributed across both nodes. Docker Swarm also provides high availability for your service. If the web server goes down on the Worker Node, then the new container will be launched on the Manager Node. To test high availability, just stop the Docker service on the Worker Node: sudo systemctl stop docker On the Manager Node, run the web server service status with the following command: docker service ps webserver You should see that a new container is launched on Manager Node:
    
    ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE            ERROR               PORTS
    ia2qc8a5f5n4        webserver.1         httpd:latest        Manager-Node        Ready               Ready 1 second ago
    7roily9zpjvq         \_ webserver.1     httpd:latest        Worker-Node         Shutdown  

## Docker Service Create

### Create a new service
Swarm This command works with the Swarm orchestrator.

## Usage

**docker service create [OPTIONS] IMAGE [COMMAND] [ARG...]**

## Description
Creates a service as described by the specified parameters.

## Examples

Create a service
  
    $ docker service create --name redis redis:3.0.6
    dmu1ept4cxcfe8k8lhtux3ro3
    
    $ docker service create --mode global --name redis2 redis:3.0.6
    a8q9dasaafudfs8q8w32udass
    
    $ docker service ls
    ID            NAME    MODE        REPLICAS  IMAGE
    dmu1ept4cxcf  redis   replicated  1/1       redis:3.0.6
    a8q9dasaafud  redis2  global      1/1       redis:3.0.6

## Create a service using an image on a private registry (--with-registry-auth)

If your image is available on a private registry which requires login, use the --with-registry-auth flag with docker service create, after logging in. If your image is stored on registry.example.com, which is a private registry, use a command like the following:
    
    $ docker login registry.example.com
    $ docker service  create \
      --with-registry-auth \
      --name my_service \
      registry.example.com/acme/my_image:latest
  
This passes the login token from your local client to the swarm nodes where the service is deployed, using the encrypted WAL logs. With this information, the nodes are able to log into the registry and pull the image.

## Create a service with 5 replica tasks (--replicas)

Use the --replicas flag to set the number of replica tasks for a replicated service. The following command creates a redis service with 5 replica tasks:

    $ docker service create --name redis --replicas=5 redis:3.0.6
    4cdgfyky7ozwh3htjfw0d12qv

The above command sets the desired number of tasks for the service. Even though the command returns immediately, actual scaling of the service may take some time. The REPLICAS column shows both the actual and desired number of replica tasks for the service.

In the following example the desired state is 5 replicas, but the current number of RUNNING tasks is 3:
    
    $ docker service ls 
    ID            NAME   MODE        REPLICAS  IMAGE
    4cdgfyky7ozw  redis  replicated  3/5       redis:3.0.7

Once all the tasks are created and RUNNING, the actual number of tasks is equal to the desired number:

    $ docker service ls
    
    ID            NAME   MODE        REPLICAS  IMAGE
    4cdgfyky7ozw  redis  replicated  5/5       redis:3.0.7

## Create a service with secrets (--secret)

Use the --secret flag to give a container access to a secret.

Create a service specifying a secret:

    $ docker service create --name redis --secret secret.json redis:3.0.6
    4cdgfyky7ozwh3htjfw0d12qv

## Create a service specifying the secret, target, user/group ID, and mode:

    $ docker service create --name redis \
        --secret source=ssh-key,target=ssh \
        --secret source=app-key,target=app,uid=1000,gid=1001,mode=0400 \
        redis:3.0.6
    4cdgfyky7ozwh3htjfw0d12qv

To grant a service access to multiple secrets, use multiple --secret flags.

Secrets are located in /run/secrets in the container if no target is specified. If no target is specified, the name of the secret is used as the in memory file in the container. If a target is specified, that is used as the filename. In the example above, two files are created: /run/secrets/ssh and /run/secrets/app for each of the secret targets specified.

## Create a service with configs (--config)

Use the --config flag to give a container access to a config.

Create a service with a config. The config will be mounted into redis-config, be owned by the user who runs the command inside the container (often root), and have file mode 0444 or world-readable. You can specify the uid and gid as numerical IDs or names. When using names, the provided group/user names must pre-exist in the container. The mode is specified as a 4-number sequence such as 0755.

    $ docker service create --name=redis --config redis-conf redis:3.0.6

Create a service with a config and specify the target location and file mode:
    
    $ docker service create --name redis \
      --config source=redis-conf,target=/etc/redis/redis.conf,mode=0400 redis:3.0.6

## To grant a service access to multiple configs, use multiple --config flags.

Configs are located in / in the container if no target is specified. If no target is specified, the name of the config is used as the name of the file in the container. If a target is specified, that is used as the filename.
Create a service with a rolling update policy

    $ docker service create \
      --replicas 10 \
      --name redis \
      --update-delay 10s \
      --update-parallelism 2 \
      redis:3.0.6

When you run a service update, the scheduler updates a maximum of 2 tasks at a time, with 10s between updates. For more information, refer to the rolling updates tutorial.
Set environment variables (-e, --env)

This sets an environment variable for all tasks in a service. For example:

    $ docker service create \
      --name redis_2 \
      --replicas 5 \
      --env MYVAR=foo \
      redis:3.0.6

To specify multiple environment variables, specify multiple --env flags, each with a separate key-value pair.

    $ docker service create \
      --name redis_2 \
      --replicas 5 \
      --env MYVAR=foo \
      --env MYVAR2=bar \
      redis:3.0.6

## Create a service with specific hostname (--hostname)

This option sets the docker service containers hostname to a specific string. For example:

    $ docker service create --name redis --hostname myredis redis:3.0.6
