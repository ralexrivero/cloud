# Docker images

```Dockerfile
██████╗  ██████╗  ██████╗██╗  ██╗███████╗██████╗
██╔══██╗██╔═══██╗██╔════╝██║ ██╔╝██╔════╝██╔══██╗
██║  ██║██║   ██║██║     █████╔╝ █████╗  ██████╔╝
██║  ██║██║   ██║██║     ██╔═██╗ ██╔══╝  ██╔══██╗
██████╔╝╚██████╔╝╚██████╗██║  ██╗███████╗██║  ██║
╚═════╝  ╚═════╝  ╚═════╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝
```

## Dockerfile

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
