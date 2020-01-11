---
title: Docker usage
date: 2020-01-11 15:56:06
categories:
	- dev
tags:
	- docker
	- usage
---

### .dockerignore

```bash
.git
```

### Dockerfile

#### build

```bash
docker build -t <image>[:<tag>] . # default tag is latest
```

<!-- more -->

### stop container(s)

```bash
docker stop <container ID> # stop specified container
# stop container which match the regex
docker stop $(docker ps | awk '/regex/ print $1') 
```

### remove container(s)

```bash
docker rm <container ID> # remove specified container
docker rm $(docker ps -a -p) # remove all containers
```

### remove image(s)

```bash
docker rmi <image> # remove specified imgae
docker rm $(docker image ls) # remove all images
```

### create a container

```bash
# create a container and run backend
docker run -d -it --name="specified-name" <image> 
```

### enter a container

```bash
docker attach <container ID> # enter a running container
```

### exit from a container

```
ctrl+p, ctrl+q
```

