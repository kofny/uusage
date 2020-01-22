---
title: Docker usage
date: 2020-01-11 15:56:06
categories:
	- dev
tags:
	- docker
	- usage
---

## 基本操作

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

## 进阶操作

### 容器互联

推荐使用网络
```bash
docker create network <network>

docker run -it --name <name1> --network <network> --network-alias <alias1> <image1>:<tag1>
docker run -it --name <name2> --network <network> --network-alias <alias2> <image1>:<tag2>
```
这样，同一网络下的两个容器可以通过别名 `--network-alias` 进行通信。

### 卷挂载

在新建容器时使用 `-v <abs-host-dir>:<abs-container-dir>` 可以让容器访问宿主机的目录，这里一定要使用绝对路径。

如果用了相对路径可能会产生不可预料的结果。可以使用
``` bash
docker inspect <container-id>
```
来查看 “Mount” 信息，可以看到挂载时的源目录与目标目录。

## Dockerfile 的编写

### COPY

COPY 在复制文件夹时，只会把文件夹下的内容复制到目标文件夹，如 hello 文件夹下有 world1.txt, good2.txt，

```Dockerfile
COPY ./hello /target
```

这样的结果是，/target 目录下会有 world1.txt 和 good2.txt 两个文件。

建议使用 `COPY ./hello /target/hello`。

另外，COPY 操作会自动创建不存在的目录。

