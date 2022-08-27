# k8s
## Kubernetes Terminology

![image](https://user-images.githubusercontent.com/59940078/187017788-a4dcf6ff-4bc8-400f-be59-c1e35186afcc.png)

## Kubernetes Pods Concept

A pod is a group of one or more containers, with shared storage/network, and a specification for how to run containers.
A Pod is basic unit of deployment.
Pods are not visible outside the cluster
Every container is wrapped around a Pod. It is possible to have more than 1 container inside a pod (extra Helper Container to process logs for example)
Kubernetes is responsible for:
- Managing the starting and stopping of containers.
- Ensuring pods are running
- Ensuring pods aren't using too much resource (Disc, CPU ...)

A Pod's IPAddress is randomly assigned.

![img](https://user-images.githubusercontent.com/59940078/168724531-cfcd779a-a73b-4626-9873-207ae35f37e9.png)

Basic Example:

![img_1](https://user-images.githubusercontent.com/59940078/168724556-9a23a6f1-6636-40ee-ad58-d6f88400b56a.png)

Meaning:

![image](https://user-images.githubusercontent.com/59940078/187019039-edd58637-a78d-43c1-a23f-99f23a7c31bf.png)

- apiVersion:
- Kind: the kind of object in this case Pod Object
- Metadata: gives pod some metadata information. (name of the pod ...)
- spec: Specifications
- containers: 
- name: name of the container
- image: Docker image (without the tag. Docker will try to pull latest image from Docker Hub (not images built locally)
- command: shell command
- args: Arguments

## Setup and Installation
- kubectl: The Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters.
- minikube: Minikube lets you run a single-node Kubernetes cluster locally

## Common command line
Start, stop, pause ... a kubernetes cluster locally.
- minikube start, minikube stop, minikube pause ...

Explore minikube ip cluster (the VM that minikube created for us)
- minikube ip

![image](https://user-images.githubusercontent.com/59940078/187020454-f4a4fcf6-4da3-4bf5-8f33-97f90803c24f.png)

Show information about the kubernetes cluster
- kubectl get all

Deploy a pod to the cluster. or to configure changes (when yaml file being edited)
- kubectl apply -f <file_name>.yaml

Delete a pod, service, replica set ... --force to forcefully terminate the pod
- kubectl delete pod <pod_name>
- kubectl delete service <service_name>
- kubectl delete rs <rs_name>

Get more information about a pod or service
- kubectl describe pod <pod_name>
- kubectl describe service <service_name>

Connect to a pod and execute command against that pod
- kubectl exec <pod_name> <command>

Connect to a pod with an interactive shell
- kubectl -it exec webapp sh

Log checking for a pod. [options] -f to freeze the log and follow as log being output
- kubectl logs <pod_name>

Port forwarding for docker installation
- kubectl port-forward <pod_name> <local_port>:<pod_port>

Status of rolling out deployment
- kubectl rollout status deploy <deploy_name>

History of rolling out deployment
- kubectl rollout history deploy <deploy_name>

Return to earlier version (Not specifying --to-revision option will automatically roll back 1 revision)
- kubectl rollout undo deploy <deploy_name> --to-revision=<revision_number>

## Services in Kubernetes
A service is a long-running object in kubernetes unlike a Pod.
With a service you can connect to the kubernetes Cluster and the service will find a suitable Pod to service that request

![image](https://user-images.githubusercontent.com/59940078/187024244-51e701a4-3618-4b36-8007-5fb3bf7582e3.png)

Pod Label:

![img_2](https://user-images.githubusercontent.com/59940078/168724595-586417fe-c473-4afa-8576-a4d26724f5c3.png)
![image](https://user-images.githubusercontent.com/59940078/168724959-19061cbb-b8d5-443b-9cc0-f5c0865478b2.png)

Meaning:
- kind: in this case pod service
- name: name of the service (very important) This will be the name where services will be able to talk to each other. PLEASE NAME APPROPRIATELY.
- selector: select which pods are going to be represented by a service. The service becomes a network endpoint for either other services or maybe external users to connect to (eg browser)
- type: nodePort (expose a service through port), ClusterIP (only for internal microservice communication)


## Deploying Zero-down time service (Amateur way)

![image](https://user-images.githubusercontent.com/59940078/187023650-3eaeec84-bc45-4737-b82c-5f96e23dbd96.png)

![image](https://user-images.githubusercontent.com/59940078/168730735-c5c32734-0940-49c5-8c3b-f876a872302b.png)

- Deploy an updated Pod
- Change release label in service to a new one that holds the new Pod (Traffic then will be automatically redirected to the new pod)

## Deploying Zero-down time service (Prefered way)

![image](https://user-images.githubusercontent.com/59940078/187023708-96024aff-6f6e-4f25-81c0-a79c7b862716.png)

## ReplicaSets
Setting Replicas will ensure the number of replicas of a container to be running at the same time.
Pod will be recreated automatically if a replica goes down.

![image](https://user-images.githubusercontent.com/59940078/168759162-4431f6f7-a78e-4f08-a944-d101dc97501b.png)

Meaning:
- template: template for the pods
- matchLabels: pretty much the same as selector (just now it is in Replica)

## Deployment

![image](https://user-images.githubusercontent.com/59940078/168823467-e4748eb6-9c16-4668-88a2-a6ae64900f8a.png)

Meaning:
- apiVersion: Deployment kind is always in apps/v1
- kind: Deployment kind (will manage replica set) (Basicly the same structure as replica set)

![image](https://user-images.githubusercontent.com/59940078/187024955-111d2081-62e9-4977-86a7-c791506ef357.png)

Rolling Deployment without manually changing the label

Be careful when rolling back to earlier deployment, yaml file won't be changed. Fixing the yaml file is necessary

Deleting a pod that is tied to a deploymenet with a replica set will recreate the pod. Also in the case if the pod fails

### Commond command line for deployment

![image](https://user-images.githubusercontent.com/59940078/187021284-d148a98a-9d89-421a-87bc-19bf213af432.png)

## Networking in Kubernetes

![image](https://user-images.githubusercontent.com/59940078/168922081-32366612-0340-43d9-9456-e5315e90395c.png)

Not recommended to deploy 2 or more containers in a single pods. Because then we would have to find out whether the pod fails because of a service or another.
Recommended Deployment Architecture (1 service per pod)

Kubernetes manage its own DNS service. Therefore, in order for a container inside a pod to communicate with another container in a different pod, containers will do a "lookup operation" (to the DNS server of kubernetes) to make network requests (kubernetes Service of Kube-dns will take care of all the ip address of different containers inside different pods)

### Example
![image](https://user-images.githubusercontent.com/59940078/169436383-406f2bbc-1f07-459d-ad33-868e00f9c553.png)

From the webapp pod, the webapp pod can make network request to another pod which contains mysql service by referencing the metadata name of : database (in the image above). The kube-dns service will resolve the database name to the correct IP in the kubernetes cluster

![image](https://user-images.githubusercontent.com/59940078/168922590-f41d8683-43d6-4303-88f6-a9c022636ca6.png)

### Namespace (Grouping pods together)

Namespace is a virtual cluster

![image](https://user-images.githubusercontent.com/59940078/168924538-a1369387-512e-40bb-9279-045a07f92851.png)

The pre-configured namespace in kubernetes is named "default". A namespace is a local network of pods.
To make network requests to a different pod in a different namespace. You have to reference the namespace of that pod in your app's URL.

![image](https://user-images.githubusercontent.com/59940078/169463837-8edcab63-91a4-43cf-85d4-328b31889f2c.png)

Getting all namespaces
- kubectl get ns

## Persistence and Volumes

Volume mount tag for pod templating.

Please consult the docs at: [k8s volumes!](https://kubernetes.io/docs/concepts/storage/volumes/) for different volumes type (every volume type has different tag)

### Sample local mount

![image](https://user-images.githubusercontent.com/59940078/169046263-f9afcd71-7e99-46c8-8c83-2f541ef62764.png)

- mountPath: this tag is the directory you want to persist in your local computer. (if the service is a database, please look through docs to find where the db store its data)

- volumes:
- hostPath: host path tag (this would be different for different cloud providers)
- path: where to persist data from the container to your local computer
- type:

![image](https://user-images.githubusercontent.com/59940078/169048203-6e64a310-85d5-445e-9bc0-1c016bfa6b7a.png)

### PersistentVolumeClaims
PVC is useful when you migrate from different cloud providers (different storage type). You won't have to hardcode configuration in your pod yaml file, instead you make a seperate yaml file to manage the persistent storage configuration

[Persistent volumes docs!](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

Example for mongodb:

![image](https://user-images.githubusercontent.com/59940078/169060702-abe43a50-f42f-4d76-954a-93ffd53a0251.png)

## StorageClasses and Binding

Command line

Get persistentVolumes:
- kubectl get pv
