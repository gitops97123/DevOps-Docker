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
