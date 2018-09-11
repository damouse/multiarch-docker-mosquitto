# Multiarch Dockerized Mosquitto

This is the backing repo for a containerized Mosquitto broker that supports `amd64` (x86_64), `arm64v8`/`aarch64`, and `armfh` (for raspberry pi).

This broker does expose websocket ports, but doesn't use the implementation provided by `libwebsockets` since I've seen serious problems 

This broker does have websockets support via a websockets proxy, but not `libwebsockets`, since @damouse saw serious stability problems when interfacing with javascript websockets library. These may have been a configuration issue or bad cosnfiguration, but the ws proxy worked out of the box.

## Running

## Building and Pushing

```
cd common
docker build . -t damouse/mosquitto:arm32v7
docker push damouse/mosquitto:arm32v7

docker build . -t damouse/mosquitto:amd64
docker push damouse/mosquitto:amd64

docker build . -t -f arm64.dockerfile damouse/mosquitto:arm64
docker push damouse/mosquitto:arm64
```

NOTE: I wasn't able to build `aarch64`, `arm64v8`, or `arm64` on my local ubuntu installation even after running the multi-arch steps detailed [here](https://www.ecliptik.com/Cross-Building-and-Running-Multi-Arch-Docker-Images/). Using `multiarch/ubuntu-core:arm64-xenial` built common, but I'm not sure if there are substantial differences between `ubuntu-core` and `ubuntu:16.04`.

Creating the manifest for multi-arch files the first time: 

```
$ docker manifest create damouse/python-common:latest damouse/python-common:arm32v7 damouse/python-common:amd64 damouse/python-common:arm64

$ docker manifest annotate damouse/python-common:latest damouse/python-common:arm32v7 --os linux --arch arm

$ docker manifest annotate damouse/python-common:latest damouse/python-common:amd64 --os linux --arch amd64

$ docker manifest annotate damouse/python-common:latest damouse/python-common:arm64 --os linux --arch arm64

$ docker manifest push damouse/python-common:latest
```

## Credits

Shoutout to (sbierman)[https://github.com/sbiermann/rpi-mosquitto/blob/master/Dockerfile], (toke)[https://github.com/toke/docker-mosquitto], and (brettmiller)[https://github.com/brettmiller/docker-mosquitto-arm64].
