
# Docker images

```Dockerfile
██████╗  ██████╗  ██████╗██╗  ██╗███████╗██████╗
██╔══██╗██╔═══██╗██╔════╝██║ ██╔╝██╔════╝██╔══██╗
██║  ██║██║   ██║██║     █████╔╝ █████╗  ██████╔╝
██║  ██║██║   ██║██║     ██╔═██╗ ██╔══╝  ██╔══██╗
██████╔╝╚██████╔╝╚██████╗██║  ██╗███████╗██║  ██║
╚═════╝  ╚═════╝  ╚═════╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝
```

## misc

- create a `Dockerfile` in the root of the project

```Dockerfile
FROM alpine
CMD ["echo", "Hello World!"]
```

- `docker build -t my-app:v1.0 .` notice the dot `.` for the current directory
- `docker run my-app:v1.0`

```bash
docker run my-app:v1.0
Hello World!
```

### Dockerfile instructions

- `FROM` - base image
- `RUN` - execute arbitrary commands
- `CMD` - default command to run, should have only one or only the last will be executed

> Every instruction creates a new layer, the more layers the bigger the image

```Dockerfile
FROM node:9.4.0-alpine
COPY app.js .
COPY package.json .
RUN npm install &&\
    apk update &&\
    apk upgrade
EXPOSE  8080
CMD node app.js
```

- `docker run -dp 8080:8080 myimage:v1`

### Dockerfile Commands

- The `FROM` instruction initializes a new build stage and specifies the base image that subsequent instructions will build upon.
- The `COPY` command enables us to copy files to our image.
- The `RUN` instruction executes commands.
- The `EXPOSE` instruction exposes a particular port with a specified protocol inside a Docker Container.
- The `CMD` instruction provides a default for executing a container, or in other words, an executable that should run in your container.

### commands

- `docker stop $(docker ps -q)` - stop all containers
- `docker rm $(docker ps -aq)` - remove all containers

### IBM registry

[IBM Registry](https://cloud.ibm.com/registry/start)

### Docker volumes

- The recommended way to presist data, stored at `/var/lib/docker/volumes/`

## basics configuration

- `version` - version of the docker-compose file
- `services` - all the containers the system must run
- `build` - the path to the Dockerfile. `.` indicates current directory of the docker-compose file

```yaml
version: '3.9'

services:
    web:
        build: .
    data:
        build: mysql
```

## Commands

- `up` - `build`, `create` and `start` the containers
- `down` - `stop`, delete all containers, images and artifacts
- `stop` - stop the containers
- `rm` - delete the containers
- `restart` - `stop` and `start`, useful for fixing random system errors

## images

> list of images

### gcc-c

> C development environment

- `gcc`
- `vim`
- `valgring`
- `betty`

> build

- `docker build -t gcc-c:1.0 .` notice the dot `.` at the end

> run

- `docker run -v /mnt/code/repos/:/repos -it --rm --name my-gcc gcc-c:1.0`

> use registry

- `docker login`
- `docker tag gcc-c:1.0 ralexrivero/gcc-c:1.0`
- `docker push ralexrivero/gcc-c:1.0`
- `docker pull ralexrivero/gcc-c:1.0`

### trusty-c

`ubuntu:trusty`
`vim`
`valgrind`
`gcc`
`g++`
`makefile`
`git`
`Betty repository`

> build

`docker tag trusty-c:1.0 ralexrivero/trusty-c:1.0`

> run

`docker run -v /mnt/code/repos/:/repos -it --rm --name my-trusty trusty-c:1.0`

> use registry

`docker tag ralexrivero/trusty-c:1.0`
`docker push ralexrivero/trusty-c:1.0`
`docker pull ralexrivero/trusty-c:1.0`

> open other prompt
`docker exec -it my-trusty bash`

### node:16-alpine - custom

- `node:16-alpine`
- `vim`
- `semistandard`

> build

`docker build -t node-alp-16:1.0 .`

> run

- `docker run -v /mnt/code/repos/:/repos --network=host -d -it --rm --user node --name node-alp-app node-alp-16:1.0`

> open prompt

- `docker exec -it my-node-alp-app sh`

### 18-apline

- `docker run -d -it --rm --name node -v /home/ralex/code:/code --publish 3000:3000 --user node node-alp:1.1`
- `docker exec -it node sh`

## Python Django

- `docker build -t python-django:1.0 .`
- `docker run -v /mnt/code/repos/:/repos --network=host -d -it --rm --name py-dj python-django:1.0`
- `docker exec -it py-dj sh`
- `docker tag python-django:1.0 ralexrivero/python-django:1.0`
- `docker push ralexrivero/python-django:1.0` push to registry
- `docker run --network`

## PostgreSql

- `docker pull postgres`
- `docker run --name postgre-app -e POSTGRES_PASSWORD=mysecretpassword -d postgres`
- `docker exec -it postgre-app psql -U postgres`

## debian slim

- `docker build -t editor .`
- `docker run --rm -ti editor` avoiding -ti will cause the keyboard to fail
