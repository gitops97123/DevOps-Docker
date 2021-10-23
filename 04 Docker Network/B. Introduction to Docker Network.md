# Basic Netwoking with Docker 

Networks can be configured to provide complete isolation for container, which enable building web applications that work together securely. 

Docker containers and Services are so powerful is that you can connect them together, or connect them to non-Docker workloads. Docker containers and services do not even need to be aware that they are deployed on Docker, or whether their peers are also Docker workloads or not. Whether your Docker hosts run Linux, Windows, or a mix of the two, you can use Docker to manage them in a platform-agnostic way. 


# Network Drivers : 

Docker's networking subsystem pluggable, using drivers. Several drivers exist by default and provide core networking functionality: 

- **bridge** 
- **host**  
- **overlay**  
- **macvlan**  
- **none** 
- **Network Plugins**  
  
### **Bridge Network**

The default network driver. If you dont's specify a driver, this is the type of network you are creating. <br>
**Bridge networks are usually used when your applications run in standalone containers that need to communicate.**


### **Host**
For standalone containers, remove network isolation between the container and the Docker host, and use the host's networking directly. 

### **Overlay**
Overlay networks connect multiple Docker daemon together and enable swarm services to communicate with each other. 

### **macvlan**
Macvlan networks allow you to assign a MAC address to a container, making it appear as a physical device on you network. The docker daemon routes traffic to containers on different Docker daemons.

### **none**
For this container, disable all networking. 

### **Network Plugins**
You can install and use third-pary network plugins with Docker. These plugins are available from Docker Hub or from third-party vendors. 

### Bridge Networking Examples : 

When you run the following command in your console, docker returns a JSON object describing the bridge network. 

    student@gitOps:~$ docker network ls
    NETWORK ID     NAME      DRIVER    SCOPE
    f6de73ca7388   bridge    bridge    local
    cf76cd94e1d1   host      host      local
    46c6f3b9de71   none      null      local

docker automatically creates a subnet and gateway for the bridge network, and `docker run` automatically adds containers to it. if you have containers running on your network, `docker network inspect` displays networking information for your containers. 

    student@gitOps:~$ docker network inspect bridge

# Define your own network 

### Creating a bridge network 

Bridge networks( similar to the default `docker 0` network ) offer the easiest solution to creating your own Docker Network. While similar, you do not simply clone the `default0` network, so you get some new features and lose some old ones. Follow along below to create your own `black_network` and run your Postgres  container `psql_db` on that network. 

    student@gitOps:~$ docker network create --driver bridge black_network 
    565564b474453b6875c018b7882ee89d64b727a3111c03156e5264cfad558598
<br>

    student@gitOps:~$ docker network inspect black_network
    [
        {
            "Name": "black_network",
            "Id": "c44fe66fc47b91b114ae6fb8cff1c78fa7dfccb68313058e9ff9fb677772ed03",
            "Created": "2021-10-22T23:59:28.410108314+05:30",
            "Scope": "local",
            "Driver": "bridge",
            "EnableIPv6": false,
            "IPAM": {
                "Driver": "default",
                "Options": {},
                "Config": [
                    {
                        "Subnet": "172.19.0.0/16",
                        "Gateway": "172.19.0.1"
                    }
                ]
            },
            "Internal": false,
            "Attachable": false,
            "Ingress": false,
            "ConfigFrom": {
                "Network": ""
            },
            "ConfigOnly": false,
            "Containers": {},
            "Options": {},
            "Labels": {}
        }
    ]

<br>

    student@gitOps:~$ docker network ls
    NETWORK ID     NAME            DRIVER    SCOPE
    c44fe66fc47b   black_network   bridge    local
    f6de73ca7388   bridge          bridge    local
    cf76cd94e1d1   host            host      local
    46c6f3b9de71   none            null      local

    student@gitOps:~$ docker pull postgres
<br>

    student@gitOps:~$ docker container run -d --network=black_network --name=psql_db  -e POSTGRES_PASSWORD=redhat postgres
    4ec49408f384749c7a98a04a09d1b7a7bc9b5b865de8a7a74b73b3a92818ff85

<br>

    student@gitOps:~$ docker container ls
    CONTAINER ID   IMAGE                   COMMAND                  CREATED         STATUS         PORTS      NAMES
    4ec49408f384   postgres                "docker-entrypoint.s…"   7 seconds ago   Up 5 seconds   5432/tcp   psql_db

    student@gitOps:~$ docker network inspect black_network
    [
        {
            "Name": "black_network",
            "Id": "c44fe66fc47b91b114ae6fb8cff1c78fa7dfccb68313058e9ff9fb677772ed03",
            "Created": "2021-10-22T23:59:28.410108314+05:30",
            "Scope": "local",
            "Driver": "bridge",
            ...
                        "Subnet": "172.19.0.0/16",
                        "Gateway": "172.19.0.1"
            ...
            "Containers": {
                "4ec49408f384749c7a98a04a09d1b7a7bc9b5b865de8a7a74b73b3a92818ff85": {
                    "Name": "psql_db",
                    "EndpointID": "f1c047fe2274c2c72719417fcaabceadce7172f5380d936e16106a721be90609",
                    "MacAddress": "02:42:ac:13:00:02",
                    "IPv4Address": "172.19.0.2/16",
                    "IPv6Address": ""
            ...
  
    student@gitOps:~$ docker stop psql_db
    psql_db

    student@gitOps:~$ docker network rm black_network
    black_network

### Creating an overlay network 

If you want native multi-host networking, you need to create an overlay network. These networks require a valid key-value store service, such as consol, Etcd, or ZooKeeper.
You must install and configure your key-value store service before creating your network.

|protocol | Port | Purpose |
|--------|------|---------|
| udp   |   4789    | data |
| tcp/udp | 7948    | contorl |


Launch containers on each host;  make sure you specify the network name: 

    student@gitOps:~$ docker run --rm -d --network host --name webserver nginx

access Nginx by browsing to http://localhost:80

Examine your network interfaces and verify that an new one was not created.

    student@gitOps:~$ ip addr show 

Verify which process is bound to port 80, using the `netstat` command. you need to use `sudo` because the process is owned by the docker daemon user and you otherwise won't be able to see its name or PID. 

     student@gitOps:~$ sudo netstat --tulpn | grep :80 

stop the container. It will be removed automatically as it was started using the `--rm` option.
    
     student@gitOps:~$ docker container stop webserver 


### Create a `macvlan` network 

create a macvlan network called `black_vlan`. Modify the subnet, gateway, and parent values to values that make sense in your environment. 

    student@gitOps:~$ docker network create -d macvlan \
                        --subnet=172.16.86.0/24 \
                        --gateway=172.16.86.1 \
                        -o parent=eth0 \
                        black_vlan 
    19df74622d8b55fbfda3571e07ef76be40bc0cd3948917063155ae14e555eab8

you can use `docker network ls` and `docker network inspect black_vlan` commands to verify that the network exists and its a `macvlan` network. 

    student@gitOps:~$ docker pull alpine:latest
    latest: Pulling from library/alpine
    a0d0a0d46f8b: Pull complete
    Digest: sha256:e1c082e3d3c45cccac829840a25941e679c25d438cc8412c2fa221cf1a824e6a
    Status: Downloaded newer image for alpine:latest
    docker.io/library/alpine:latest

    student@gitOps:~$ docker run --rm -dit --network black_vlan --name alpine_container alpine:latest ash 
    e37b65cb505f1db43e338eb9e03d260f5f80c609adfe1bb4cac1f5f214f0fbcf

inspect the `black_vlan` container and notice the `MacAddress` key within the `Networks` key: 

    student@gitOps:~$ docker network inspect black_vlan 
    [
        {
            "Name": "black_vlan",
            "Id": "19df74622d8b55fbfda3571e07ef76be40bc0cd3948917063155ae14e555eab8",
            "Created": "2021-10-23T00:32:47.705581984+05:30",
            "Scope": "local",
            "Driver": "macvlan",
            "EnableIPv6": false,
            "IPAM": {
                "Driver": "default",
                "Options": {},
                "Config": [
                    {
                        "Subnet": "172.16.86.0/24",
                        "Gateway": "172.16.86.1"
                    }
                ]
            },
            "Internal": false,
            "Attachable": false,
            "Ingress": false,
            "ConfigFrom": {
                "Network": ""
            },
            "ConfigOnly": false,
            "Containers": {
                "e37b65cb505f1db43e338eb9e03d260f5f80c609adfe1bb4cac1f5f214f0fbcf": {
                    "Name": "alpine_container",
                    "EndpointID": "4486545b1b5c4ae507183cddc60700e1a5636aef46f810ba08ccd786ebe3b569",
                    "MacAddress": "02:42:ac:10:56:02",
                    "IPv4Address": "172.16.86.2/24",
                    "IPv6Address": ""
                }
            },
            "Options": {
                "parent": "ens33"
            },
            "Labels": {}
        }
    ]

Inspect the `alpine_container` container 

    student@gitOps:~$ docker container inspect alpine_container
    
    ... truncated...
                "Networks": {
                "black_vlan": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": [
                        "e37b65cb505f"
                    ],
                    "NetworkID": "19df74622d8b55fbfda3571e07ef76be40bc0cd3948917063155ae14e555eab8",
                    "EndpointID": "4486545b1b5c4ae507183cddc60700e1a5636aef46f810ba08ccd786ebe3b569",
                    "Gateway": "172.16.86.1",
                    "IPAddress": "172.16.86.2",
                    "IPPrefixLen": 24,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:10:56:02",
                    "DriverOpts": null
                }
            }

execute the container and checkout the `MacAddress` and Ip Address. 

    student@gitOps:~$ docker exec alpine_container ip addr show eth0
    52: eth0@if2: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
        link/ether 02:42:ac:10:56:02 brd ff:ff:ff:ff:ff:ff
        inet 172.16.86.2/24 brd 172.16.86.255 scope global eth0
        valid_lft forever preferred_lft forever

    student@gitOps:~$ docker exec alpine_container ip route
    default via 172.16.86.1 dev eth0
    172.16.86.0/24 dev eth0 scope link  src 172.16.86.2

stop the container (Docker removes it becaues of the `--rm` flag), and remove the network. 

    student@gitOps:~$ docker container stop alpine_container 
    student@gitOps:~$ docker network rm black_vlan 
