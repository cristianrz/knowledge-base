# Docker

## Build container from dockerfile

```shell
$ docker build -t [tag] .
```

## Run container 

* Run container detached and exit when the root process used to run the container exits

```shell
$ docker run -d [image] [daemon]
```

* Run container detached and attach a terminal

```shell
$ docker run -d -t [image] [command]
```

* Run container interactively

```shell
$ docker run -it [image] [command]
```

## Execute process in running container

```shell
$ docker exec -it [container] [command]
```


## Export filesystem

```shell
$ docker export [container] > rootfs.tar
```

## Stop and remove all containers

```shell
$ docker stop "$(docker ps -a -q)"
$ docker rm "$(docker ps -a -q)"
```

## Dockerfile

```Dockerfile
FROM debian

RUN apt update
RUN apt install curl -y
RUN cd && curl "https://raw.githubusercontent.com/dylanaraps/promptless/master/install.sh" | sh

CMD /bin/bash -l
```
