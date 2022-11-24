# Docker images

```Dockerfile
██████╗  ██████╗  ██████╗██╗  ██╗███████╗██████╗
██╔══██╗██╔═══██╗██╔════╝██║ ██╔╝██╔════╝██╔══██╗
██║  ██║██║   ██║██║     █████╔╝ █████╗  ██████╔╝
██║  ██║██║   ██║██║     ██╔═██╗ ██╔══╝  ██╔══██╗
██████╔╝╚██████╔╝╚██████╗██║  ██╗███████╗██║  ██║
╚═════╝  ╚═════╝  ╚═════╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝
```

## run

Containers have a main process, the container stops when that proces stops

### --rm

Remove the container when it stops

- `docker run --rm -it alpine sh`

### sleep

- `docker run --rm -it alpine sleep 5`
- sleeps for 5 seconds and then stops

## -c "command"

- `docker run --rm -it alpine sh -c "sleep 5; echo bye!"`
- sleeps for 5 seconds and then prints bye!

## ti

- `docker run -ti alpine sh` terminal interactive mode

## detached

- `docker run --rm -d alpine sleep 5`
- runs in the background

- `ctl + p + q`
- detach from the container without stopping it

## attach

- `docker attach <container_id>`
- attach to a running container

## exec

- `docker exec -it <container_id> sh`
- attach to a running container
- if the original container is stopped, this will stop as well

## ps

- `docker ps` - list running containers
- `docker ps -a` - list all containers
- `docker ps -l` - list last created container
- format the output
- `docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}\t{{.Size}}"`

```sh
CONTAINER ID   NAMES            IMAGE           STATUS         PORTS     SIZE
8d1a1888e316   stoic_einstein   alpine:latest   Up 8 minutes             8B (virtual 5.54MB)
```

- `docker ps --format $FORMAT`
- `docker ps -a --format $FORMAT`

```sh
export FORMAT="\nID\t{{.ID}}\nIMAGE\t{{.Image}}\nCOMMAND\t{{.Command}}\nCREATED\t{{.RunningFor}}\nSTATUS\t{{.Status}}\nPORTS\t{{.Ports}}\nNAMES\t{{.Names}}\n"
```

```sh
ID	8d1a1888e316
IMAGE	alpine:latest
COMMAND	"sh"
CREATED	10 minutes ago
STATUS	Up 10 minutes
PORTS
NAMES	stoic_einstein
```

## commit

create image from container

- `docker commit container_id` basic usage
- `docker commit -m "message" -a "author" container_id image_name:tag`

## tag

- `docker tag image_id image_name:tag`

## logs

- `docker logs container_name` - show logs from container

## kill

- `docker kill container_name` - kill a running container

## docker rm

- `docker rm container_name` - remove a container

## Resource constraints

- `docker run --rm -it --cpus 0.5 alpine sh` - limit cpu usage
- `docker run --memory maximum-allowed-memory image-name command` - limit memory usage
- `docker run --cpu-shares` - limit cpu usage
- `docker run --cpu-quota` - limit cpu usage

## publish

- publish a port to expose it to the host
- `docker run --rm -ti -p 45678 -p 45679 alpine sh` - publish ports and docker will choose an dinamical asigned port
- `docker port container_name` - show port mappings
- can explicit the protocol UDP/TCP
- `docker run -p 45678:45678/udp`

## Container networking

- `docker network ls`

- `bridge` - default network
- `host` - use host network stack, has security concerns
- `none` - no networking

- `docker network create network_name` - create a network
- `docker network create --driver bridge network_name` - create a network with a specific driver
- `docker network inspect network_name` - inspect a network
- put containers into the network
- `docker run --rm -it --network network_name alpine --name server01 sh`
- `docker run --rm -it --network network_name alpine --name server02 sh`
- from server01
- `ping server02`

## legacy linking

> Legacy linking works by connecting ports on one machine to all the ports on the other machine

- only works in one direction
- avoid using it
- `docker run --rm -it -e SECRET=123 --name mysql mysql:5.7`
- `-e` - set environment variable
- `docker run --rm -it --link mysql:db alpine sh` - link mysql container to alpine container

## images

- `docker images` - list images
- `docker images ls` - list images
- `docker rmi image_name` - remove image

## volumes

- share files or directories between host and container
- `docker run -ti -v /home/code:/shared-folder alpine sh` - mount a volume

- ephemeral volumes
- volumes from
- `docker run -v /shared-data --name sharing alpine sh`
- `docker run -ti --volumes-from sharing alpine sh` - mount a volume from another container

## registry

- search for images

- `docker search image_name` - search for images
- `docker login` - login to docker hub
- `docker tag image_name username/image_name:tag` - tag an image
- `docker push username/image_name:tag` - push image to docker hub

## build

> build images from `Dockerfile`

- `docker build -t image_name:tag .` - build image from Dockerfile, the dot denotes the current directory, or replace with the path to the Dockerfile

> every step in the Dockerfile is cached to speed up the build process

```Dockerfile
FROM busybox
RUN echo "Building simple docker image"
```

### statements

#### FROM

- what image use to start running from
- allways the first statement

#### MAINTAINER

- who maintains the image
- MAINTAINER Firstname Lastname <email@example.com>

#### RUN

- run a command, waits it to finish, and saves the result
- RUN <command>

#### ADD

- copy files from the host to the container
- ADD <src> <dest>
- add the contents of tar archives, automatically uncompress in the destination
- add remote files using URLs
- ADD https://example.com/file.tar.gz /tmp/

#### ENV

- set environment variables
- both during the build and when running the result
- ENV DB_PORT=5432

#### ENTRYPOINT

- specify the start of the command to run
- everything typed when run image would be treated as arguments of the command

#### CMD

- specify the whole command to run, can be overwritten when running the image
- the arguments replace the CMD command

#### EXPOSE

- expose a port to the host
- EXPOSE <port>

#### VOLUME

- create a mount point for a volume
- VOLUME <path> for ephemeral volumes
- VOLUME <path>:<path> for persistent volumes

#### WORKDIR

- set the working directory
- WORKDIR <path>

#### USER

- set the user to run the commands
- USER <user

[https://docs.docker.com/engine/reference/builder/](https://docs.docker.com/engine/reference/builder/)

## Multi-stage builds

- multi stage reduce drastically the size of the image

```Dockerfile
FROM ubuntu:16.04
RUN apt-get update
RUN apt-get -y install curl
RUN curl https://google.com | wc -c > google-size
ENTRYPOINT echo google is this big; cat google-size
```

- Dockerfile is splitted into named parts and copied to the same path, which preserves the ability to have a reproducible build

```Dockerfile
FROM ubuntu:16.04 as builder
RUN apt-get update
RUN apt-get -y install curl
RUN curl https://google.com | wc -c > google-size

FROM alpine
COPY --from=builder /google-size /google-size
ENTRYPOINT echo google is this big; cat google-size
```

## Process

- `docker inspect --format '{{.State.Pid}}' container_name` - get the process id of a container

## Namespaces

- namespaces provides isolation between the host and the running containers

- `ps -ef` - show all processes
- `ip addr` - show all network interfaces

## enables on boot

- `systemctl status docker` - check if docker is running
- `sudo systemctl enable docker` - enable docker on boot
- `sudo systemctl disable docker` - disable docker on boot

## backign up Docker

- Docker swarm cluster
- Universal Control Plane
- Docker Trusted Registry
- Container volume data

- `systemctl stop docker` - stop docker
- `cd /var/lib/docker` - go to docker data directory
- `tar -czvf docker-backup.tar.gz .` - create a backup
- `cp docker-backup.tar.gz /home/backup` - copy backup to another location
- `tar -xzvf docker-backup.tar.gz` - restore backup
- `systemctl start docker` - start docker

## backing up UCP

- use the oficial documentation to get the command

## backing up DTR

- back up DTR metadata

## Container volume Data

- `docker volume ls` - list volumes

## Docker status

- `docker version` - if shows the version, docker can talk to the server
- `docker info` - show docker info
- `docker ps` - show running containers
- `docker ps -a` - show all containers running and stopped
- `docker node ls` - show nodes in the swarm, only valid if running on a manager
- `docker service ls` - show services running in the swarm, only valid if running on a manager

## loggin for prduction

- `docker info | grep 'Logging Driver'
- `docker logs container_name` - show logs on a specific container
- `docker logs -f container_name` - show logs on a specific container and follow
- `docker run --log-driver=syslog --log-opt syslog-addres=udp://1.1.1.1. alpine` - run a container with syslog driver, and send logs to the syslog server
