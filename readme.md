# Kubernetes in a Nutshell

## Architecture

<table><tr><td>
<img align="center" src="./pics/overview.png" title="Architectual Overview" width="300">
</td></tr></table>

### Cluster, Master and Nodes

<table><tr><td>
<img align="center" src="./pics/cluster.png" title="Cluster" width="300">
</td></tr></table>



<table><tr><td>
<img align="center" src="./pics/k8s_deployment.png" title="Deplyoment with Master" width="300">
</td></tr></table>


## Pods - _"It's all about Pods"_
A Pod is an atomic unit of deployment in K8S.

1. Virtualization does VM's
2. Docker does containers
3. _**Kubernetes does Pods**_

### Pods vs. Containers
- containers run in Pods (>= 1 container per Pod)
- Pods are a shared execution environment for one or more containers
  - shared resources are IP addresses, ports, hostname, sockets, memory, volumes, routing table etc.
- kind of  wrapper around a container or a special kind of containers
- multi-containers Pods useful for >1 containers sharing resources i.e. logging and service meshes
- more common are single-container Pods

### Microservices and Pods
One concern for one container i.e. one container for web-service and one container for file-sync service

### Deployment of Pods

- to deploy we define a _manifest file_ and post that file to the API server

### Intra-Pod communication

- every Pod can talk directly to every other Pod
- two containers in one Pod communicate via _localhost_ interface

### Pods and Control Groups (cgroups)
- _cgroups_ stop conatiners to consume all available CPU, RAM and IOPS on a node (Kind of Police)
- each container within a Pod can have their own cgroup limits


## Deployment
- Deployments bring self-healing, scalability, rolling updates and rollbacks
- manages just a set of identical Pods
  - Pod-1 for frontend => Deployment-1
  - Pod-2 for backend => Deployment-2
- Deployments leverages another object called **ReplicaSet**
- **ReplicaSet** manage Pods

_**kind: Deployment**_
  ```
  controllers/nginx-deployment.yaml  
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: nginx-deployment
    labels:
      app: nginx
  spec:
    replicas: 3
    selector:
      matchLabels:
        app: nginx
    template:
      metadata:
        labels:
          app: nginx
      spec:
        containers:
        - name: nginx
          image: nginx:1.15.4
          ports:
          - containerPort: 80
  ```
Creating a Deployment:
```
  kubectl create -f nginx-deployment.yaml
  kubectl get deployments
```




### Self-Healing and scalability

#### Declarative Model
- Desired state - Desired state is what you want.
- Current state - Current state is what you have.

The Declarative model is a way of telling Kubernetes what our desired state is, without getting into the detail of how to implement it. Kubernetes is constantly checking whether _current state_ matches _desired state_.

## Services

TBD
