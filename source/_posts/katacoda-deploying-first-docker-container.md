---
title: katacoda-deploying-first-docker-container
date: 2019-05-16 12:40:16
tags: docker
---

# Running a container background(detached)
**docker search image-name**
```
	docker search redis
```
**docker run options image-name:version**
**solution1: default latest version**
```
	docker run -d redis
```
**solution2: version is 3.2** 
```
	docker run -d redis:3.2
```
# Find running container
```
	docker ps
	docker inspect <container-id>
	docker logs <container-id>
```
# Access redis
	note: each docker container is **sandboxed**
**solution1: specify host port is 6379**
*-p host-port:container-post* 
```
docker run -d --name redisHostPort -p 6379:6379 redis:latest
```
**solution2: specify randomly host port**
*-p container-port* 
```
docker run -d --name redisDynamic  -p 6379 redis:latest
docker port redisDynamic 6379
```
# Persisting data
*-v host-dir:container-dir*
```
	docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis
```
# Running a container foreground
```
	docker run -it ubuntu bash
```
