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
