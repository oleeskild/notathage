---
{"dg-publish":true,"permalink":"/ressurser/snippets/docker-cheatsheet/"}
---

# Docker Cheatsheet


# Building from dockerfile 
**In context of folder containing a file called Dockerfile**
```
docker build .
```

**Specify filename**
```
docker build -f MyDockerfile .
```

**Build with custom tag**
```
docker build -f MyDockerfile -t my-service-image .
```


# Starting and stopping

- `[docker start](https://docs.docker.com/engine/reference/commandline/start)` starts a container so it is running.
- `[docker stop](https://docs.docker.com/engine/reference/commandline/stop)` stops a running container.
- `[docker restart](https://docs.docker.com/engine/reference/commandline/restart)` stops and starts a container.
- `[docker pause](https://docs.docker.com/engine/reference/commandline/pause/)` pauses a running container, "freezing" it in place.
- `[docker unpause](https://docs.docker.com/engine/reference/commandline/unpause/)` will unpause a running container.
- `[docker wait](https://docs.docker.com/engine/reference/commandline/wait)` blocks until running container stops.
- `[docker kill](https://docs.docker.com/engine/reference/commandline/kill)` sends a SIGKILL to a running container.
- `[docker attach](https://docs.docker.com/engine/reference/commandline/attach)` will connect to a running container.

# **Clusters (docker-compose.yml)**

- `docker-compose up --build -d` rebuilds the docker containers specified in the docker-compose.yml file and starts it as a daemon