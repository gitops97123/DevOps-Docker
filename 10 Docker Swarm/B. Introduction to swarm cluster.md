

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

