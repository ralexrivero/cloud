# Docker Compose

## install

- `sudo apt update && sudo apt -y full-upgrade`
- `sudo apt install curl gnupg2 apt-transport-https software-properties-common ca-certificates`
- `[ -f /var/run/reboot-required ] && sudo reboot -f`
- `curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker-archive-keyring.gpg`
- `echo "deb [arch=amd64] https://download.docker.com/linux/debian bullseye stable" | sudo tee  /etc/apt/sources.list.d/docker.list`
- `sudo apt update`
- `sudo apt install docker-ce docker-ce-cli containerd.io`

## yaml

- yaml stand for Yet Another Markup Language

- `docker-compose.yml`

- `version` : version of docker-compose
- `services` : services to run, use indentation to define services
  - `service_name` : name of service
    - `build`: build image from Dockerfile, specify path to Dockerfile, use `.` for current directory
    - or use `image`: use image to fetch automatically from docker hub

## run

- `docker-compose up` : run all services described below:
  - `build`
  - `create`
  - `start`
