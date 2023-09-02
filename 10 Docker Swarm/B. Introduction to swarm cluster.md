

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
