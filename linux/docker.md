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

## Good reaping init file

```bash
#!/bin/sh  
  
set -eu  
  
USER=user  
LOG_FILE="/var/log/file.log"  
  
cleanup(){  
       #printf '=> Received SIGTERM, reaping procesess... '  
  
       kill -15 "$PROCESS1" || true  
       kill -15 "$PROCESS2" || true  
       wait  
       #echo OK  
  
       #echo '=> All done, exiting'  
       exit 0  
}  
  
#printf '=> Setting up trap...'  
trap 'cleanup' 15  
#echo OK  
  
#printf '=> Start process1 in the background... '  
/bin/su - "$USER" -c "exec process1" &  
PROCESS1="$!"  
#echo OK  
  
#printf '=> Starting process2 process... '  
/bin/su - "$USER" -c "exec process" &  
PROCESS2="$!"  
#echo OK  
  
#echo '=> Startup finished.'  
  
wait
```

## Good Dockerfile

```Dockerfile
FROM docker.io/debian:stable-slim  

RUN apt-get -q update \  
       && apt-get -y dist-upgrade \  
       && apt-get -q install -y --no-install-recommends apt-utils curl iproute2 openresolv procps qbittorrent-nox util-linux  
wireguard-tools \  
       && rm -rf /var/lib/apt/lists/* \  
       && useradd -m qbit \  
       && chown -R qbit:qbit /home/qbit  

COPY ./init /init  

WORKDIR /home/qbit  

ENTRYPOINT ["/init"]  

EXPOSE 8080  

HEALTHCHECK CMD pgrep qbittorrent-nox && curl -v gnu.org
```

## User docker with podman socket

```bash
systemctl --user enable --now podman.socket
export DOCKER_HOST=unix:///run/user/$UID/podman/podman.sock
```
