# Kubernetes Test

## Requirements

* Docker
* Minikube

## Steps

### 1. Clone the github repository

```bash
git clone https://github.com/ellamfar/docker-streaming-server.git
cd docker-streaming-server
```

### 2. Create a minikube cluster

Use Minikube to get started with the kubernetes development. Minikube is a lightweight Kubernetes implementation that creates a virtual machine on your local machine and deploys a simple cluster containing only one node.

```bash
minikube start --mount --mount-string "$(pwd):/docker-streaming-server/"
```

The deployments that will be launched to Kubernetes depend on the directories and files in the repository. Therefore, I used the ``--mount`` and ``--mount-string`` arguments to mount the repository into minikube. This way, the repository will be available on minikube for the pods and deployments to access.

Check that the ``/docker-streaming-server`` directory was created in the minikube cluster by entering the cluster and listing the directories in it.

```bash
minikube ssh
df -hl
```

### 3. Create the deployments, services and persistent volume claims

I used the declerative method of creating a Kubernetes deployment. This means that I created YAML files to describe the desired deployment state.

The [``deployment-server.yaml``](deployment-server.yaml) file launches the streaming server Kubernetes deployment.

The [``deployment-consumer.yaml``](deployment-consumer.yaml) file launches a deployment that allows us to view the stream live on a webpage.

Inside the two YAML files, you will find individual sections describing the deployment, the service, the persistent volume and the persistent volume claim.

Deployments provides declarative updates for Pods and ReplicaSets.

Services are an abstract way of exposing applications running on a set of Pods as a network service.

Persistent Volumes (PV) are pieces of storage in the cluster that have been provisioned by an administrator or dynamically provisioned using Storage Classes.

Persistent Volume Claims (PVC) are requests for storage by a user.

To launch the deployments, run the following commands:

```bash
kubectl apply -f deployment-server.yaml 
kubectl apply -f deployment-consumer.yaml
```
### 4. Expose the ``LoadBalancer`` services using minikube tunnel

``minikube tunnel`` runs as a process, creating a network route on the host to the service CIDR of the cluster using the cluster’s IP address as a gateway. The tunnel command exposes the external IP directly to any program running on the host operating system.

```bash
minikube tunnel
```

### 5. Find the external IP addresses

Run the following command:

```bash
kubectl get services
```

Note down the EXTERNAL-IP addresses of the streaming-consumer and streaming-server services.

### 5. Stream the content

* Use [Open Broadcaster Software](https://obsproject.com/) to stream your content:
    * Add media source ``Sources`` > ``+`` > ``Video Capture Device``
    * Configure streaming server ``Controls`` > ``Settings`` > ``Stream``
        * Stream Type: ``Custom Streaming Server``
        * URL: ``rtmp://<EXTERNAL_IP_STREAMINGSERVER>:1935/live``
        * Stream Key: ``test``
    * Press ``Start Streaming`` button.
* Go to ``http://<EXTERNAL_IP_STREAMINGCONSUMER>:9999/``


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
