## Container orchestration / Container management

Tutorial: [https://www.youtube.com/watch?v=F-p_7XaEC84](https://www.youtube.com/watch?v=F-p_7XaEC84)

## why

tldr: You have no autoscaling or load balancing of any sort using only docker. Kubernetes provides a simple way to configure load balancers and when and how your app should scale.

Since the trend to switch from Monolith applications to microservices there has been an increasing demand for tools like docker since containers are the perfect place for a microservice. These applications usualy require multiple containers that need to be able to communicate between each other and scale based on load. Setting these up manually and servicing them upon crashes or other problems is not really feasable. This is where orchestrators come in. An orchestrator is a tool that takes care of setting up containers, scales them up when needed, makes it easy to ensure high availability and fault tolerance.

## Required features

1. High availability - strive to 100% up time
1. Scalability - scale based on load for high performannce
1. Disaster recovery - backup and restore, you can rerun a node with the latest date before the crash

## Others - not going to go into detail

- Swarm - original solution by docker
- MESOS

# Kubernetes

Open source container orchestration framework by Google.

## Structure

1. Pod - an abstraction above containers to allow different implementations ( docker or other ). Usually only one container runs inside a Pod but running multiple is possible. Each Pod gets an IP address from the virtual network.
1. Node - simple phyisical or virtual machine. Pods run in a Node. Can be master or worker.
1. Cluster - set of Nodes

## Components

1. API server - the frontend of kubernetes
1. etcd server - distributed reliable key value store - keeps the state
1. Scheduler - distributes containers across nodes
1. Controllers - responsible for noticing and responding to load / crashes
1. container runtime - underlying software that runs containers ( could be docker )
1. kubelet - kubernetes agent that runs on each node

## kubectl

Kubernetes cli.

`kubectl run <name>` - runs a cluster
`kubectl cluster-info <name>` - gets detailed info about cluster
`kubectl get nodes <name>` - gets info on nodes on the cluster

## Virtual Network

This allows the nodes to talk to each other.
