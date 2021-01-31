# Docker Containers

## How Docker works


## Useful commands

Run a docker container

`docker run`

# Kubernetes

Kubernetes automates the distribution and scheduling of application containers across a cluster in a more efficient way.

### Cluster Diagram

![](images/kube_cluster_1.PNG)

**Master** - responsible for managing and coordinating the cluster

**Node** - VM or physical computer that serves as a worker machine in a Kubernetes cluster that run applications

Nodes communicate with master using the Kubernetes API

## Kubernetes Deployment

Create a Kubernetes **Deployment** configuration.  
* Instructs how to create and update instances of application
* *Kubernetes Deployment Controller* continuously monitors application instances (if node down, it will start up another instance in another node)

**Create a Deployment**  
`kubectl create deployment <name> --image=<image>`  
What it does:  
* Search for a suitable node to run the app instance
* Schedule the application to run on the node
* Configure cluster to reschedule instance on a new Node when needed


**List Deployments**  
`kubectl get deployments`


**View running application**  
Pods running inside Kubernetes are in a private, isolated network. They are visible from other pods and services within the same Kubernetes cluster, not outside, by default.

Create a proxy to forward requests into the private network:  
`kubectl proxy`

## Pods and Nodes
![](./images/pods_overview.PNG)  
### Pod
> Atomic unit on Kubernetes.
* a group of one or more application containers (such as Docker) and some shared resources for those containers (e.g. shared storage, networking, specific ports to use)
* Containers in a Pod share an IP address and port space, always co-located and co-scheduled, and run in a shared context on the same Node
* Deployment creates Pods with containers inside them (not create the containers directly)
* Each Pod is tied to the Node where it is scheduled

### Node
![](./images/node_overview.PNG)
* Worker machine in Kuberentes (virtual or physical)
* Each Node is managed by the Master
* A Node can have multiple pods
* Each Node runs at least
  * Kubelete - process responsible for communication between Master and Node; manages the Pods and containers
  * A container runtime (e.g. Docker) - responsible for pulling container image, unpacking the container and running the application

**Troubleshooting with kubectl**

`kubectl get` - list resources (verify application deployed is running)  
* `kubectl get pods`


`kubectl describe` - show information about a resource 
* `kubectl describe pods` - details about Pod's container (e.g. UP address, ports used)

`kubectl logs` - print logs from a container in a pod  
`kubectl exec` - execute a command on a container in a pod


## Useful `kubectl` commands

View cluster details:  
`kubectl cluster-info`

View cluster nodes:  
`kubectl get nodes`