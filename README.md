# Multiarch Dockerized Mosquitto

This is the backing repo for a containerized Mosquitto broker that supports `amd64` (x86_64), `arm64v8`/`aarch64`, and `armfh` (for raspberry pi).

This broker does expose websocket ports, but doesn't use the implementation provided by `libwebsockets` since I've seen serious problems 

This broker does have websockets support via a websockets proxy, but not `libwebsockets`, since @damouse saw serious stability problems when interfacing with javascript websockets library. These may have been a configuration issue or bad configuration, but the ws proxy worked out of the box.

## Building and Pushing

