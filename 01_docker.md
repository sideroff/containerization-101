# docker

Tutorial: [https://www.youtube.com/watch?v=fqMOX6JJhGo](https://www.youtube.com/watch?v=fqMOX6JJhGo)

## why

`Matrix from hell or compatibility matrix` - it happens when your application has a lot of different dependencies that each need to be compatible between themselves and also be supported by the OS. It is difficult to maintain the same version of each of those dependencies and the os on each environment and team member. Thus a more robust method is required.

You can avoid that problem by running each of your dependencies in an isolated container. This way the context in which your application is ran is always explicit and strict. (eg. no mismatch in node version inbetween developers & environments)

## docker hub/store

[https://hub.docker.com/](https://hub.docker.com/)
A registry of all public images. You can publish your own image.

## image

A template for the context of the container. You can specify os type, prerequisites to be installed, script to be ran upon start, etc. The most common setups should be already available in the docker hub ( eg. basic linux distro with only node version X installed - node:X-alpine )

## image tags ( versions )

`<image name>:<version name>`
default version name is latest

## docker cli

Well documented using --help anywhere.

### management cmds

1. `builder` Manage builds
1. `config` Manage Docker configs
1. `container` Manage containers
1. `context` Manage contexts
1. `image` Manage images
1. `network` Manage networks
1. `node` Manage Swarm nodes
1. `plugin` Manage plugins
1. `secret` Manage Docker secrets
1. `service` Manage services
1. `stack` Manage Docker stacks
1. `swarm` Manage Swarm
1. `system` Manage Docker
1. `trust` Manage trust on Docker images
1. `volume` Manage volumes

Each of these supports commands of its own. To view the supported commands and flags for any management command use —-help (eg. `docker —help`; `docker container —help`, etc...)

## stdin & stdout

by default containers are **NOT** run in interactive mode. To allow the docker container to read stdin, supply `--interactive` or `-i` to run
for stdout supply `--tty` or `-t`
for both `-it`

### viewing the stdout & stderr of a detached component

`docker logs <container id>`

## port mapping

Ports inside a container are not accessible outside. For that to work you need to map a host port ( port on the one running the containerization tool ) to the corresponding port in the container. Supply `-p` or `--publish` <host port>:<container port> to the run command to publish the port.

## volume mapping

Data inside the docker container dies when the container is removed. To save data, you can map a independant volume to a volume inside the container. This way, all the data that would otherwise live inside the container gets written to the volume. Supply `-v` or `--volume <volume path>:<container path>` to the run command to map the volume.

## detailed container data

`docker inspect <container id>` => json

## environment variables

Supply `-e` or `—env <env_name>=<env_value>` to the run command.
Environment variables that a container has set can be inspected by running the inspect command for a specific container

## building an image

`docker build <path to Dockerfile> -t <tag>`

## publish an image

`docker push <tag>`

## Dockerfile

`<instruction> <argument>`
Instructions are all caps.

1. `FROM` - first line (can be preceeded by variables using ARG), specifies the base docker image
1. `RUN` - runs argument in the bash
1. `COPY <foo> <bar>` - copies everything from host foo directory to container bar directory
1. `ENTRYPOINT` - commands to run when image is run as conntainer

### Dockerfile layers

instructions are “layers” as in they are hashed and cached and provide an incremental history of the ran instructions

### CMD vs ENTRYPOINT

CMD is replaced by parameters supplied to run.
ENTRYPOINT prepended to parameters supplied to run.

## Network

By default, Docker creates 3 networks when installed: Bridge ( default ), Host & None. Usefull info on which exact network a container is under can be seen using the inspect command for that container.

### Bridge

A VPC created by docker that every container lives in by default, to access outside net, use port mapping. You can create custom bridge networks using the `docker network create` command.

### Host

The container is ran inside the host network so no port mapping is needed.

### None

No attachments to any network.

### Specifyingn a network on container creation

`--network=<host|none>`

## Storage

Docker stores all data in `/var/lib/docker/`

## Resources

Docker has a sort of alias for every process that it starts. This alias pid is linked to an actual process id on the host machine. Thus it's clear that docker containers use the underlying os kernel.
