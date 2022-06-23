
# Docker images

```Dockerfile
██████╗  ██████╗  ██████╗██╗  ██╗███████╗██████╗
██╔══██╗██╔═══██╗██╔════╝██║ ██╔╝██╔════╝██╔══██╗
██║  ██║██║   ██║██║     █████╔╝ █████╗  ██████╔╝
██║  ██║██║   ██║██║     ██╔═██╗ ██╔══╝  ██╔══██╗
██████╔╝╚██████╔╝╚██████╗██║  ██╗███████╗██║  ██║
╚═════╝  ╚═════╝  ╚═════╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝
```

## Environment

[![Ubuntu](https://img.shields.io/static/v1?label=&message=Ubuntu&color=E95420&logo=Ubuntu&logoColor=E95420&labelColor=2F333A)](https://ubuntu.com/)<!-- ubuntu -->
[![Ubuntu](https://img.shields.io/static/v1?label=&message=Kali%20Linux&color=557C94&logo=Kali%20Linux&logoColor=557C94&labelColor=2F333A)](https://www.kali.org/)<!-- kali linux -->
[![Bash](https://img.shields.io/static/v1?label=&message=GNU%20Bash&color=4EAA25&logo=GNU%20Bash&logoColor=4EAA25&labelColor=2F333A)](https://www.gnu.org/software/bash/)<!-- bash -->
[![Vim](https://img.shields.io/static/v1?label=&message=Vim&color=019733&logo=Vim&logoColor=019733&labelColor=2F333A)](https://www.vim.org/)<!-- vim -->
[![VS Code](https://img.shields.io/static/v1?label=&message=Visual%20Studio%20Code&color=007ACC&logo=Visual%20Studio%20Code&logoColor=007ACC&labelColor=2F333A)](https://code.visualstudio.com/)<!-- vs code -->
[![Git](https://img.shields.io/static/v1?label=&message=Git&color=F05032&logo=Git&logoColor=F05032&labelColor=2F333A)](https://git-scm.com/)<!-- git -->
[![Github](https://img.shields.io/static/v1?label=&message=GitHub&color=181717&logo=GitHub&logoColor=f2f2f2&labelColor=2F333A)](https://github.com)<!-- github -->
[![Docker](https://img.shields.io/static/v1?label=&message=Docker&color=2496ED&logo=Docker&labelColor=2F333A)](https://hub.docker.com)<!-- docker -->

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

`docker build -t node-16-alp:1.0 .`

> run

- `docker run -v /mnt/code/repos/:/repos -it --rm --name my-node-16-alp node-16-alp:1.0`

> open prompt

- `docker exec -it my-node-16-alp sh`

## Author

<!-- twitter -->
[![Twitter](https://img.shields.io/twitter/follow/ralex_uy?style=social)](https://twitter.com/ralex_uy) <!-- linkedin --> [![Linkedin](https://img.shields.io/badge/LinkedIn-+24K-blue?style=social&logo=linkedin)](https://www.linkedin.com/in/ronald-rivero/) <!-- github --> [![Github](https://img.shields.io/github/followers/ralexrivero?style=social)](https://github.com/ralexrivero/) <!-- vagrant --> [![Vagrant](https://img.shields.io/static/v1?label=&message=Vagrant%20Profile&color=1868F2&logo=vagrant&labelColor=2F333A)](https://app.vagrantup.com/ralexrivero) <!-- docker --> [![Docker](https://img.shields.io/static/v1?label=&message=Docker%20Profile&color=2496ED&logo=Docker&labelColor=2F333A)](https://hub.docker.com/u/ralexrivero)
