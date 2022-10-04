# Containers

## Docker file

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
