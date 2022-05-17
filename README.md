# k8s

## Kubernetes Pods Concept

A pod is a group of one or more containers, with shared storage/network, and a specification for how to run containers.
A Pod is basic unit of deployment.
Pods are not visible outside the cluster
Every container is wrapped around a Pod. It is possible to have more than 1 container inside a pod (extra Helper Container to process logs for example)
Kubernetes is responsible for:
- Managing the starting and stopping of containers.
- Ensuring pods are running
- Ensuring pods aren't using too much resource (Disc, CPU ...)

![img](https://user-images.githubusercontent.com/59940078/168724531-cfcd779a-a73b-4626-9873-207ae35f37e9.png)

Basic Example:

![img_1](https://user-images.githubusercontent.com/59940078/168724556-9a23a6f1-6636-40ee-ad58-d6f88400b56a.png)

Meaning:
- apiVersion:
- Kind: the kind of object in this case Pod Object
- Metadata: gives pod some metadata information. (name of the pod ...)
- spec: Specifications
- containers: 
- name: name of the container
- image: Docker image
- command: shell command
- args: Arguments

## Setup and Installation
- kubectl: The Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters.
- minikube: Minikube lets you run a single-node Kubernetes cluster locally

## Command line
Start, stop, pause ... a kubernetes cluster locally.
- minikube start, minikube stop, minikube pause ...

Explore minikube ip cluster (the VM that minikube created for us)
- minikube ip

Show information about the kubernetes cluster
- kubectl get all

Deploy a pod to the cluster
- kubectl apply -f <file_name>.yaml

Get more information about a pod
- kubectl describe pod <pod_name>

Connect to a pod and execute command against that pod
- kubectl exec <pod_name> <command>

Connect to a pod with an interactive shell
- kubectl -it exec webapp sh

Port forwarding for docker installation
- kubectl port-forward <pod_name> <local_port>:<pod_port>

## Services in Kubernetes
A service is a long-running object in kubernetes unlike a Pod.
With a service you can connect to the Kubernetes Cluster and the service will find a suitable Pod to service that request

Pod Label:

![img_2](https://user-images.githubusercontent.com/59940078/168724595-586417fe-c473-4afa-8576-a4d26724f5c3.png)
![image](https://user-images.githubusercontent.com/59940078/168724959-19061cbb-b8d5-443b-9cc0-f5c0865478b2.png)







