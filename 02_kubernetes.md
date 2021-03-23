## Container orchestration / Container management

Tutorial: [https://www.youtube.com/watch?v=F-p_7XaEC84](https://www.youtube.com/watch?v=F-p_7XaEC84)

## why

tldr: You have no autoscaling or load balancing of any sort using only docker. Kubernetes provides a simple way to configure load balancers and when and how your app should scale.

Since the trend to switch from Monolith applications to microservices there has been an increasing demand for tools like docker since containers are the perfect place for a microservice. These applications usualy require multiple containers that need to be able to communicate between each other and scale based on load. Setting these up manually and servicing them upon crashes or other problems is not really feasable. This is where orchestrators come in. An orchestrator is a tool that takes care of setting up containers, scales them up when needed, makes it easy to ensure high availability and fault tolerance.

## Required features

1. High availability - strive to 100% up time
1. Scalability - scale based on load for high performannce
1. Disaster recovery - backup and restore, you can rerun a node with the latest date before the crash
1. Load balancing - no reason to have multiple containers if you cannot utilize them

## Features

1. Service discovery and load balancing - Kubernetes can expose containers using dns names. This allows it to automaticlaly discover new instances. It also monitors traffic and load balances based on your config.
1. Storage orchestration - Kubernetes allows automatic storage mounting for local or cloud storages.
1. Automated rollouts and rollbacks - Kubernetes can controll the rollouts of your new containers, keeping high up time. Additionally, it can automatically rollback if any problems occur.
1. Automated bin packing - Given the available resources and requirements for each container, Kubernetes can provision these containers in it's Nodes such that your resources are utulized to the max.
1. Self healing - Restarts containers that fail. Kills any zombie containers and replaces them with new ones. You can have a user defined health check.
1. Secret and configuration management - Manages secrets without the need to redeploy.

## Others - not going to go into detail

- Swarm - original solution by docker
- MESOS

# Kubernetes

Open source container orchestration framework by Google.

## Structure

1. Cluster - set of Nodes, each Cluster has atleast one Node.
1. Control plane - Container orchestration layer ( Manager ) of all Nodes in the cluster. It exposes the API and controls the Cluster. Can be ran across multiple computers.
1. Node - simple phyisical or virtual worker machine. each Node can have more than one Pod.
1. Pod - an abstraction above containers to allow different implementations ( docker or other ). Usually only one container runs inside a Pod but running multiple is possible. Each Pod gets an IP address from the virtual network.

## Control Plane Components

1. Kubernetes API server - The frontend of Kubernetes.
1. etcd - distributed reliable key value store - keeps the state of the Cluster
1. Scheduler - Watches for newly created Pods and distributes them across Nodes
1. Controller
    1.1. Node controller - Keeps track of crashed Nodes
    1.1. Replication controller - Maintains the correct number of Pods in a Node
    1.1. Endpoints controller - Populates the Endpoints object ( joins Services and Pods )
    1.1. Service Account & Token controllers - Manage accounts and API access tokens for new namespaces

## Node Components

1. Kubelet - Kubernetes agent that runs on every Node. Allows communication and monitoring of that Node.
1. kube-proxy - Network proxy that makes sure the network configuration is followed
1. Container runtime - for instance, Docker

## Addons

You can use Addons to achieve additional features such as: DNS, Container monitoring, Cluster-level logging, etc.

## kubectl

Kubernetes cli.

`kubectl run <name>` - runs a cluster
`kubectl cluster-info <name>` - gets detailed info about cluster
`kubectl get nodes <name>` - gets info on nodes on the cluster

## Virtual Network

This allows the nodes to talk to each other.

# Pod Lifecycle

1. `Pending` - During start up time.
1. `Running` - If atleast one of its primary Containers starts properly.
1. `Succeeded`/`Failed` - Depending on the exit code of Containers.
1. `Unknown` - Undetermined, most probably a communication error.

# Container lifecycle

1. `Waiting` - During start up time.
1. `Running` - Started properly. `PostStart` hook has been executed by this point.
1. `Terminated` - Either ran to completion or failed. `PreStop` hook has been executed by this point.

# Container lifecycle hooks

Simmilar to React / Angular component lifecycle hooks.

1. `PostStart` - Executed immediately after container creation.
1. `PreStop` - Executed immediately before a container is terminated.

# Updates and Rollbacks

Kubernetes allows progressive updates in order to ensure no down time. It also will rollback to a previous state if update fails.

# Networking

Kubernetes networking addresses four concerns:

1. Communication between Containers in a Pod
1. Communication between Pods 
1. Exposing applications to the outside of the cluster ( reachable from the outside )
1. Exposing applications to the inside of your cluster ( only reachable inside )

# Service

Say you have 1-N Pods that need to talk to each other. Since Pods are dynamically created and deleted by Kubernetes, there is no elegant solution for them to keep track of one another. A Service is a way for developers to specify a logical set of Pods with a "name" that can later be used to communicate with them. This means that Kubernetes is free to manage resources by creating and destroying certain Pods, but that does not break the communication between them.

Services can be Internal and External. 

## Internal
Internal services are used for communication between pods in a deployment.
## External
External ones are used to expose endpoints.
## Ingress
Since external services are in some 
```
<host>:<port>
```
format, Kubernetes has a thing called Ingerss which an external service goes through and ingress makes it available from a url (http://mywebsite.com)



# Configuration
Common things like endpoints, secrets, tokens don't need to be in your application code. Rather you can use ENV vars to expose them to your application. ConfigMap and Secrets can both be exposed using ENV vars.
## ConfigMap
External configuration of your application ( endpoints, etc. )

## Secret
Just like configmap, but base64 encoded and used to store secrets.


# Storage
Storage is by default ephemeral. Using volumes can avoid the data loss on pod deletion.


# Deployment
 A blueprint for an application ( pod types, service types, secrets, replicas, etc..). Usually configured by a yaml/json file. Creating a deployment ( kubectl create deployment ) automatically creates a replicaset and a pod managed by it. In most cased you only need to manage deployments and kubernetes will manage everything below them for you. It checks a desired state agains the real state and issues commands based on any discrepencies. 

 # StatefulSet
 A deployment-like feature designed so that reads and writes are synchronized, useful for dbs.

# Working with configuration files
You can create everything in a deployment from the command line, but since the whole idea is to have setups as code, kubernetes can read a config from a file with the following command:
```
kubectl apply -f YAML_CONFIGURATION_FILE.yaml
```

If you change the yaml file and rerun the apply command, kubectl will look for a deployment with the same name as the one specified in the file and will decide whether to create or shut down the old and start the new one.

## Configuration file components
You can look at nginx-deployment as a reference.

### Required Fields

apiVersion - Which version of the Kubernetes API you're using to create this object
kind - What kind of object you want to create
metadata - Data that helps uniquely identify the object, including a name string, UID, and optional namespace
spec - What state you desire for the object

### Customization
Information about the format of each specific one can be found [here](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/) -> Kubernetes API Reference

# Debugging 
 ```
 kubectl exec -it DEPLOYMENT_NAME -- [bin/sh bin/bash]
 ```

 # Common commands
 [kubectl cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

