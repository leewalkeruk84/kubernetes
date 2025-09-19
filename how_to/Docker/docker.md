# Docker

# Using Containers
## Starting Containers
```bash
docker run nginx
docker run --name=web nginx
```
### Starting a container so that it runs in the background
```bash
docker run -d nginx
docker run -d --name=web nginx
```

## Starting a container with a interactive terminal
```bash
docker run -i -t busybox
docker run --name=temp -i -t busybox
```

## See running containers
```bash
docker ps
```

## See all containers, even the ones that have stopped
```bash
docker ps -a
```

## Remove containers
> **Note**: you can only remove a stopped container, so use the stop command first
> ```bash
> docker ps -a
> docker stop '<conainer-id> | <container-name>'
> docker rm '<conainer-id> | <container-name>'
> docker ps -a
> ```


## Start a stopped container
```bash
docker start '<conainer-id> | <container-name>'
```
## Show properties of a currently running container
```bash
docker inspect '<conainer-id> | <container-name>'
```

## To see container logs
```bash
docker logs '<conainer-id> | <container-name>'
```

# Managing Container Images

## See a list of local container images
```bash
docker images
```

## Search the remote repo for image with a search term, like nginx
```bash
docker search nginx
```

## Retrieve a image from the remote repo
```bash
docker pull alpine
```

## Remove unused images 
```bash
docker image prune
```

## Build a image from a Dockerfile
> . specfies the current directory, can place any path where Dockerfile is found
> -t (tag) specifies the name of the image you want to build from the Dockerfile
```bash
docker build -t '<imagename>' .
docker build -t alpine .
docker build -t nginx:alpine .
```
> **Example**: From the current directory with a Dockerfile located
> Also a countdown script within the same dir
>```bash
> cat Dockerfile
> cat countdown
> docker build -t myapp:1.0.0 .
> docker images
> docker image inspect myapp:1.0.0
> docker run -d --name=leesapp myapp:1.0.0
> docker ps
> docker logs leesapp
> docker logs '<container-id>'
>```

## Build a image from a running container, that changes have been made to
## 
```bash
Docker commit '<runningContainerName>' '<name:tag>'
```
> **Example**: 
> create newapp container and then create a new file in it
>```bash
> docker run --name newapp --it nginx sh
> touch /tmp/testfile
> exit
> Now use docker commit to create a new image from the running container
>```
> docker commit newapp nginx:customimage
> docker images
> docker run -i -t --name custom nginx:customimage sh
> ls /tmp
> exit
>```bash