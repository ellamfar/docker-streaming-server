# Kubernetes Test

## Requirements

* 
### 1. Clone the repository

Clone github repository and enter it.

```bash
git clone https://github.com/ellamfar/docker-streaming-server.git
cd docker-streaming-server
```

### 2. 
Start off by creating a minikube cluster.

```bash
cd docker-streaming-server
minikube start --mount --mount-string "$(pwd):/docker-streaming-server/"
```

```bash
minikube ssh
df -hl
```

```bash
kubectl apply -f deployment-server.yaml 
kubectl apply -f deployment-consumer.yaml
```

```bash
minikube tunnel
```


----------------------------------------------------------

[![Circle CI](https://circleci.com/gh/codeworksio/docker-streaming-server.svg?style=shield "CircleCI")](https://circleci.com/gh/codeworksio/docker-streaming-server)&nbsp;[![Size](https://images.microbadger.com/badges/image/codeworksio/streaming-server.svg)](http://microbadger.com/images/codeworksio/streaming-server)&nbsp;[![Version](https://images.microbadger.com/badges/version/codeworksio/streaming-server.svg)](http://microbadger.com/images/codeworksio/streaming-server)&nbsp;[![Commit](https://images.microbadger.com/badges/commit/codeworksio/streaming-server.svg)](http://microbadger.com/images/codeworksio/streaming-server)&nbsp;[![Docker Hub](https://img.shields.io/docker/pulls/codeworksio/streaming-server.svg)](https://hub.docker.com/r/codeworksio/streaming-server/)

Docker Streaming Server
=======================

A robust way of streaming media content live using the [NGINX](https://nginx.org/) web server and its [RTMP](https://github.com/tiangolo/nginx-rtmp-docker) module.

Installation
------------

Builds of the image are available on [Docker Hub](https://hub.docker.com/r/codeworksio/streaming-server/).

    docker pull codeworksio/streaming-server

Alternatively you can build the image yourself.

    docker build --tag codeworksio/streaming-server \
        github.com/codeworksio/docker-streaming-server

Quickstart
----------

Start container using:

    docker run --detach --restart always \
        --name streaming-server \
        --hostname streaming-server \
        --publish 1935:1935 \
        --publish 8080:8080 \
        --publish 8443:8443 \
        codeworksio/streaming-server

Example
-------

1. Start the streaming server and consumer from the command line

    ```bash
    cd ./documents/examples
    docker-compose up -d
    ```

2. Use [Open Broadcaster Software](https://obsproject.com/) to stream your content

    * Add media source `Sources` > `+` > `Video Capture Device`
    * Configure streaming server `Controls` > `Settings` > `Stream`
        - Stream type: `Custom Streaming Server`
        - URL: `rtmp://localhost/live`
        - Stream key: `test`
    * Press `Start Streaming` button

3. Go to `http://localhost:9999` URL address in your browser to view the media live.

See
---

* [Open Broadcaster Software post](https://obsproject.com/forum/resources/how-to-set-up-your-own-private-streaming-server-server-using-nginx.50/)
