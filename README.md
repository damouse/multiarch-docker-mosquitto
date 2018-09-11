# Multiarch Dockerized Mosquitto

This is the backing repo for a containerized Mosquitto broker that supports `amd64` (x86_64), `arm64v8`/`aarch64`, and `armfh` (for raspberry pi).

This broker does expose websocket ports, but doesn't use the implementation provided by `libwebsockets` since I've seen serious problems 

This broker does have websockets support via a websockets proxy, but not `libwebsockets`, since @damouse saw serious stability problems when interfacing with javascript websockets library. These may have been a configuration issue or bad cosnfiguration, but the ws proxy worked out of the box.

## Running

## Building and Pushing

```
docker build . -f armhf.dockerfile  -t damouse/mosquitto:armhf
docker push damouse/mosquitto:armhf

docker build . -f amd64.dockerfile -t damouse/mosquitto:amd64
docker push damouse/mosquitto:amd64

docker build . -f arm64.dockerfile -t  damouse/mosquitto:arm64v8
docker push damouse/mosquitto:arm64v8
```

Creating the manifest for multi-arch files the first time: 

```
$ docker manifest create damouse/mosquitto:latest damouse/mosquitto:armhf damouse/mosquitto:amd64 damouse/mosquitto:arm64v8

$ docker manifest annotate damouse/mosquitto:latest damouse/mosquitto:armhf --os linux --arch arm

$ docker manifest annotate damouse/mosquitto:latest damouse/mosquitto:amd64 --os linux --arch amd64

$ docker manifest annotate damouse/mosquitto:latest damouse/mosquitto:arm64v8 --os linux --arch arm64v8

$ docker manifest push damouse/mosquitto:latest
```

## Credits

Shoutout to (sbierman)[https://github.com/sbiermann/rpi-mosquitto/blob/master/Dockerfile], (toke)[https://github.com/toke/docker-mosquitto], and (brettmiller)[https://github.com/brettmiller/docker-mosquitto-arm64].
