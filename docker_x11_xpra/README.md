
## docker-x11-xpra

A docker image for development with common build dependencies, proving a basis for X applications accessible via ssh with or without Xpra.

This component was inspired by prior work from:
https://github.com/retog/docker-x11-xpra

Image based on [buildpack-deps](https://hub.docker.com/_/buildpack-deps/):stretch.

## Usage example

#### Launch container
Build image 

    docker build -t x11-xpra .

Run with

    docker run -p 2020:22 -d --name x11-xpra x11-xpra 

The container is available through the SSH at 127.0.0.1:2020 with credentials user:user.

#### Attention!!! SSH password access is allowed.

#### Start host application
##### Windows:
Start Xpra application and connect to container user@127.0.0.1:2020 :100