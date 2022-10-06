
# Docker images

```Dockerfile
██████╗  ██████╗  ██████╗██╗  ██╗███████╗██████╗
██╔══██╗██╔═══██╗██╔════╝██║ ██╔╝██╔════╝██╔══██╗
██║  ██║██║   ██║██║     █████╔╝ █████╗  ██████╔╝
██║  ██║██║   ██║██║     ██╔═██╗ ██╔══╝  ██╔══██╗
██████╔╝╚██████╔╝╚██████╗██║  ██╗███████╗██║  ██║
╚═════╝  ╚═════╝  ╚═════╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝
```

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

## Python Django

- `docker build -t python-django:1.0 .`
- `docker run -v /mnt/code/repos/:/repos --network=host -d -it --rm --name py-dj python-django:1.0`
- `docker exec -it py-dj sh`
- `docker tag python-django:1.0 ralexrivero/python-django:1.0`
- `docker push ralexrivero/python-django:1.0` push to registry
- `docker run --network

## PostgreSql

- `docker pull postgres`
- `docker run --name postgre-app -e POSTGRES_PASSWORD=mysecretpassword -d postgres`
- `docker exec -it postgre-app psql -U postgres`
