---
layout: page
title: Docker Cheatsheet
permalink: /docker-cheatsheet/
parent: Miscellaneous
nav_order: 5
---

Source: <https://collabnix.com/docker-cheatsheet/>

# Docker Cheatsheet

## Basic Docker CLIs

- `docker run`: run a command in a new container
- `docker build`: build an image from a Dockerfile
- `docker push`: push an image to a registry
- `docker pull`: pull an image from a registry
- `docker stop`: stop a running container
- `docker rm`: remove one or more containers

## Container management

- `docker create IMAGE [COMMAND]`: create a container
- `docker run IMAGE [COMMAND]`: create and start a container
- `docker start CONTAINER...`: start one or more stopped containers
- `docker stop CONTAINER...`: stop one or more running containers (graceful)
- `docker kill CONTAINER...`: kill one or more running containers (SIGKILL)
- `docker restart CONTAINER...`: restart one or more containers
- `docker pause CONTAINER...`: suspend processes in a container
- `docker unpause CONTAINER...`: resume a paused container
- `docker wait CONTAINER...`: block until containers stop, then print exit codes
- `docker top CONTAINER...`: display running processes of a container
- `docker rm [-f] CONTAINER...`: remove containers (`-f` removes running containers too)

## Inspecting containers

- `docker ps`: list running containers
- `docker ps -a`: list all containers
- `docker logs [-f] CONTAINER`: show container output (`-f` to follow)
- `docker diff CONTAINER`: show filesystem changes compared to the image
- `docker top CONTAINER...`: show processes in the container
- `docker inspect --format=... CONTAINER`: inspect and extract fields with `--format`

{% raw %}
```bash
docker inspect --format='{{ .Config.Image }}' CONTAINER
```
{% endraw %}

## Interacting with containers

- `docker attach CONTAINER`: attach to a running container (stdin/stdout/stderr)
- `docker exec CONTAINER ARGS...`: run a command in an existing container (useful for debugging)
- `docker cp CONTAINER:PATH HOSTPATH`: copy files from the container
- `docker cp HOSTPATH CONTAINER:PATH`: copy files into the container
- `docker export CONTAINER > container.tar`: export container filesystem as a tar archive
- `docker wait CONTAINER...`: wait until the container terminates and return exit code
- `docker commit CONTAINER IMAGE`: create an image from a container (snapshot)

## Image management

- `docker images`: list local images
- `docker history IMAGE`: show image history (ancestors)
- `docker inspect IMAGE...`: show low-level info (JSON)
- `docker tag IMAGE TARGET_IMAGE[:TAG]`: tag an image
- `docker rmi IMAGE...`: remove image(s)
- `docker import URL|- [TAG]`: create an image from a tarball

## Image transfer

- `docker push REPO[:TAG]`: push an image to a registry
- `docker pull REPO[:TAG]`: pull an image from a registry
- `docker search TEXT`: search images on Docker Hub
- `docker login`: log in to a registry
- `docker logout`: log out from a registry
- `docker save REPO[:TAG] > image.tar`: save an image/repo to a tarball
- `docker load < image.tar`: load images from a tarball

## Docker Compose

### Basic example

```yaml
version: '2'
services:
  web:
    build:
      context: ./Path
      dockerfile: Dockerfile
    ports:
      - "5000:5000"
    volumes:
      - .:/code
  redis:
    image: redis
```

## Docker services (Swarm)

- `docker service ls`: list services
- `docker stack services STACK_NAME`: list services in a stack
- `docker service logs STACK_NAME_SERVICE_NAME`: view service logs
- `docker service scale STACK_NAME_SERVICE_NAME=REPLICAS`: scale a service

## Docker clean

- `docker image prune`: prune unused (dangling) images
- `docker image prune -a`: remove all images not used by any container
- `docker system prune`: prune unused images, containers, networks, volumes
- `docker swarm leave`: leave a swarm
- `docker stack rm STACK_NAME`: remove a stack
- `docker kill $(docker ps -q)`: kill all running containers

## Docker security (Scout, SBOM)

Docker Scout can analyze images/artifacts for vulnerabilities and SBOM.

- `docker scout cves [OPTIONS] IMAGE | DIRECTORY | ARCHIVE`: show vulnerabilities
- `docker scout cves redis.tar`: example for an archive
- `docker scout cves --format sarif --output redis.sarif.json redis`: export to SARIF
- `docker scout compare --to redis:6.0 redis:6-bullseye`: compare images
- `docker scout quickview redis:6.0`: quick overview

## Troubleshooting

- `docker system df`: show Docker disk usage
- `docker desktop diagnose`: run Docker Desktop diagnostics