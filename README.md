
# Docker images

# gcc-c

`docker build -t gcc-c:1.0 .`

`docker run -v /mnt/code/repos/:/repos -it --rm --name my-gcc gcc-c:1.0`

`docker login`
`docker tag gcc-c:1.0 ralexrivero/gcc-c:1.0`
`docker push ralexrivero/gcc-c:1.0`
