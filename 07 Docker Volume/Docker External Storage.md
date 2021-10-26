# Docker External Storage
	
When containers are removed, data in them are also lost, so it's necessary to use external filesystem in containers as persistent storages if you need.

It's possible to mount a directory on Docker Host into containers.

    student@gitOps:~$ sudo mkdir -p /var/lib/docker/disk01
    student@gitOps:~$ sudo echo "i love docker" >> /var/lib/docker/disk-A/file.txt
    student@gitOps:~$ docker run -it -v /var/lib/docker/disk01:/mnt ubuntu /bin/bash
    root@c2bf4bfa9980:/# df -hT
    Filesystem                        Type     Size  Used Avail Use% Mounted on
    overlay                           overlay   27G    7G   20G  10% /
    tmpfs                             tmpfs     64M     0   64M   0% /dev
    tmpfs                             tmpfs    7.9G     0  7.9G   0% /sys/fs/cgroup
    shm                               tmpfs     64M     0   64M   0% /dev/shm
    /dev/mapper/ubuntu--vg-ubuntu--lv ext4      27G    7G   20G  10% /mnt
    tmpfs                             tmpfs    7.9G     0  7.9G   0% /proc/acpi
    tmpfs                             tmpfs    7.9G     0  7.9G   0% /proc/scsi

    root@c2bf4bfa9980:/# cat /mnt/testfile.txt
    i love docker

It's also possible to configure external storage by Docker Data Volume command.

    student@gitOps:~$ docker volume create volume01
    volume01

display volume list

    student@gitOps:~$ docker volume ls
    DRIVER    VOLUME NAME
    local     volume01

display details of [volume01]

    student@gitOps:~$ docker volume inspect volume01
    [
        {
            "CreatedAt": "2021-10-12T12:23:43Z",
            "Driver": "local",
            "Labels": {},
            "Mountpoint": "/var/lib/docker/volumes/volume01/_data",
            "Name": "volume01",
            "Options": {},
            "Scope": "local"
        }
    ]    

    student@gitOps:~$ docker run -it -v volume01:/mnt ubuntu
    root@f7d1e8c1ec6c:/# df -hT /mnt
    Filesystem                        Type  Size  Used Avail Use% Mounted on
    /dev/mapper/ubuntu--vg-ubuntu--lv ext4   27G  7G   20G   10%  /mnt
    root@f7d1e8c1ec6c:/# echo "Docker Volume test" > /mnt/testfile.txt
    root@f7d1e8c1ec6c:/# exit

    student@gitOps:~$ cat /var/lib/docker/volumes/volume01/_data/testfile.txt
    Docker Volume test

 possible to mount from other containers

    student@gitOps:~$ docker run -v volume01:/var/volume01 ubuntu /usr/bin/cat /var/volume01/testfile.txt
    Docker Volume test

to remove volumes, do like follows

    student@gitOps:~$ docker volume rm volume01
    
if some containers are using the volume you'd like to remove like above,
it needs to remove target containers before removing a volume

    student@gitOps:~$ docker rm f7d1e8c1ec6c2513812cae796e3b0ceb5483dd10da4b22110ca2e973c3349ca5
    student@gitOps:~$ docker rm 4ff0c7a6155cff247140680ced1452d57d1e2e388f2910532266e80bd2c41285
    student@gitOps:~$ docker volume rm volume01