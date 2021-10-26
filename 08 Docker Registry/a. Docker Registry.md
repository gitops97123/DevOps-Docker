## Install Docker-Registry to build Private Registry for Docker images.

## Install docker-registry. 

    student@gitOps:~$ sudo apt install docker-registry

## Configure Registry Setting:- 

This is the settings to use HTTP connection and no-authentication.
    
    student@gitOps:~$ vi /etc/docker/registry/config.yml

    version: 0.1
    log:
    fields:
        service: registry
    storage:
    cache:
        blobdescriptor: inmemory
    filesystem:
        rootdirectory: /var/lib/docker-registry
    delete:
        enabled: true
    http:
    addr: :5000
    headers:
        X-Content-Type-Options: [nosniff]
    #auth:
    #  htpasswd:
    #    realm: basic-realm
    #    path: /etc/docker/registry
    health:
    storagedriver:
        enabled: true
        interval: 10s
        threshold: 3

start docker-registry service. 

    student@gitOps:~$ sudo systemctl restart docker-registry

verify possible to access from any clients
 for HTTP connection, it needs to add [insecure-registries] setting

    student@gitOps:~$ vi /etc/docker/daemon.json

    # add hosts to allow HTTP connection
    {
    "insecure-registries":
        [
        "docker.internal:5000",
        "gitops.example.com:5000"
        ]
    }

    student@gitOps:~$ sudo systemctl restart docker-registry 

[push] from localhost

    student@gitOps:~$ docker tag nginx gitops.example.com:5000/nginx:my-registry
    student@gitOps:~$ docker push gitops.example.com:5000/nginx:my-registry
    student@gitOps:~$ docker images
    gitops.example.com:5000/nginx                      latest      98660e6e4c3a   2 weeks ago         128MB
    nginx                                              latest      87a94228f133   2 weeks ago         133MB
    gitops.example.com:5000/nginx                      my-registry e64579b7d886   5 weeks ago         128MB
    k8s.gcr.io/kube-controller-manager                 v1.22.2     5425bcbd23c5   5 weeks ago         122MB
    k8s.gcr.io/kube-scheduler                          v1.22.2     b51ddc1014b0   5 weeks ago         52.7MB

[pull] from another node

    student@gitOps:~$ docker pull gitops.example.com:5000/nginx:my-registry 
    student@gitOps:~$ docker images
    gitops.example.com:5000/nginx:my-registry          latest      98660e6e4c3a   2 weeks ago         128MB
    nginx                                              latest      87a94228f133   2 weeks ago         133MB
    gitops.example.com:5000/nginx:my-registry          my-registry e64579b7d886   5 weeks ago         128MB
    k8s.gcr.io/kube-controller-manager                 v1.22.2     5425bcbd23c5   5 weeks ago         122MB
    k8s.gcr.io/kube-scheduler                          v1.22.2     b51ddc1014b0   5 weeks ago         52.7MB

To enable Basic authentication, Configure like follows.

    student@gitOps:~$ sudo apt -y install apache2-utils apache2 
    student@gitOps:~$ sudo systemctl restart apache2 
    student@gitOps:~$ sudo vi /etc/docker/registry/config.yml
    # uncomment [auth] section and specify passwd file
    .....
    .....
    auth:
    htpasswd:
        realm: basic-realm
        path: /etc/docker/registry/.htpasswd
    .....
    .....

    student@gitOps:~$ sudo systemctl restart docker-registry

add users
add [-c] at initial file creation

    student@gitOps:~$ sudo htpasswd -Bc /etc/docker/registry/.htpasswd ubuntu
    New password: ubuntu
    Re-type new password: ubuntu
    Adding password for user ubuntu 

verify possible to access

    student@gitOps:~$ docker pull gitops.example.com:5000/nginx:my-registry 
    Error response from daemon: Head http://gitops.example.com:5000/v2/nginx/manifests/my-registry: no basic auth credentials
 
 authenticate by a user added with [htpasswd]
  
    root@worker1:~# docker login gitops.example.com:5000
    Username: ubuntu
    Password:
    WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
    Configure a credential helper to remove this warning. See
    https://docs.docker.com/engine/reference/commandline/login/#credentials-store

    Login Succeeded

    root@worker1:~# docker pull gitops.example.com:5000/nginx:my-registry
    root@worker1:~# docker images
    REPOSITORY                      TAG           IMAGE ID       CREATED       SIZE
    ubuntu                          latest        7e0aa2d69a15   2 weeks ago   72.7MB
    gitops.example.com:5000/nginx   my-registry   62d49f9bab67   4 weeks ago   133MB

