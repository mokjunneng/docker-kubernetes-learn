
What's the difference between a mutable and an immutable system.

- artifact that we create and the record of how we created it
- old image can be used for rollback if an error occurs

Declarative configuration

- idea of storing declarative configuration in source control is referred as infrastructure as code
- imperative instructions rarely include the reverse instructions for rollback

Decoupling

- each component separated from other components by defined API and service load balancers
  - APIs provide a buffer between implementer and consumer
  - Load balancers provide a buffer between running instances of each service

Easy to build decoupled microservices
- pods (groups of container) can group together container images developed by different teams into a single deployable unit
- k8s services - load balancing, naming and discovery to isolate one microservice from another
- namespaces - isolation, access control
- ingress - combine multiple microservices into a single externalized API surface area

Separates development from specific machine (containerization)
High degree of portability (abstract from cloud service, infrastructure)
Ensure high level of utilization of machine - CPU time

Creating & Running Containers

How to build application container images?
- Application programs comprised of languag runtime, libraries, source code, external libraries (shared components in OS)
- Use Docker (container runtime engine) to package an executable and push it to a remote registry
  - Solve problems of dependency management - immutable images and infrastructure
- Container image
  - binary package that encapsulates all of the files necessary to run a program inside of an OS container

Container layering
- container images constructed with a series of filesystem layers, where each layer inherits and modifies the layers that came before it

Container configuration file
- provides instructions on how to set up the container environment and execute an application entry point

Containers - system vs application
- system container mimic VM and runs a full boot process, includes a set of system services such as `ssh`, `syslog`
- application container just run a single program


Dockerfile
- order layers from least likely to change to most likely to change in order to optimize image size for pushing and pulling

Multi-stage builds
- build image - contains compiler, toolchains, source code
- deployment image - contains compiled binary

Remote registry
- public or private registry
  - private requires authentication
  - public allows anyone to download images
- each cloud has their own container registry

Docker container runtime
- set up application container using container-specific APIs native to the target OS

Limit resource usage
- Limit memory
  - `--memory`, `--memory-swap` flags
- Limit CPU
  - `--cpu-shares`

Cleanup
- `docker rmi <tag-name>`, `docker rmi <image-id>`


## Deploy Kubernetes Cluster

local development - minikube (only creates a single-node cluster)
cloud-based development - managed service by AWS, Azure, Google

### Cluster components
- deployed using Kubernetes itself
- they run in the kube-system namespace

Kubernetes Proxy
- routes network traffic to load-balanced services in the cluster
- present on every node (in a container)
- setup by DaemonSet

Kubernetes DNS
- naming and discovery for services defined in cluster
- also runs as a replicated service (multiple servers in cluster)
- runs as a Kubernetes deployment

Kubernetes UI
- GUI interface to explore cluster, create new containers

## Common kubectl commands

Namespaces
- organise objects in cluster (each namespace is like a folder)
- default namespace is `default`, pass `--namespace` flag to change; pass `--all-namespaces` flag to see all objects

Contexts
- change default namespace permanently
- recorded in kubectl configuration file (`$HOME/.kube/config`)
- create a context with different default namespace
  - `kubectl config set-context my-context --namespace=mystuff`
  - run `kubectl config use-context my-context` to use the newly created context
- Can be used to manage diff clusters or diff users for authenticating to those clusters using `--users` or `--clusters` with `set-context`

View Kubernetes API Objects
- RESTful
  - each object exists at a unique HTTP path (e.g. https://your-k8s.com/api/v1/namespaces/default/pods/my-pod)
  - kubectl command makes HTTP requests to these URLs to access the K8s objects
- `kubectl get <resource-name>`
  - list all resources in the current namespace
- `kubectl get <resource-name> <obj-name>`
  - get specific resource
- view more details
  - `-o` flag, to view as JSON/YAML - use `-o json` or `-o yaml`
- remove headers - `--no-headers`
- extract specific fields from object, uses the JSONPath query language
  - e.g. `kubectl get pods my-pod -o jsonpath --template={.status.podIP}`
- more details information
  - `kubectl describve <resource-name> <obj-name>`

Create, Update, Destroy Kubernetes Objects
- For objects stored in yaml files
  - create: `kubectl apply -f obj.yaml`
  - update: `kubectl apply -f obj.yaml` (if make changes to object)
- To see what `apply` command will do without making actual changes
  - `--dry-run` flag
- `apply` command records history of previous configurations in an annotation within object
  - manipulate records with `edit-last-applied`, `set-last-applied`, `view-last-applied` (e.g. `kubectl apply -f myobj.yaml view-last-applied`)
- delete object
  - `kubectl delete -f obj.yaml`
  - delete using resource type and name: `kubectl delete <resource-name> <obj-name>`


Labelling and Annotating objects
- add label/annotation
  - `kubectl label <resource-name> <label-name> <label>` (e.g. `kubectl label pods bar color=red`)
- `label` and `annotate` will not overwrite existing label by default
  - use `--overwrite` flag
- remove label
  - use `<label-name>-` syntax (e.g. `kubectl label pods bar color-`)

Debugging
- `kubectl logs <pod-name>`
  - add `-f` flag to continuously stream the logs
- choose container to view in Pod: `-c` flag
- execute a command in running container
  - `kubectl exec -it <pod-name> -- bash`
- attach to running process, allow to send input assuming process is set up to read from stdin
  - `kubectl attach -it <pod-name>`
- copy files to and from a container
  - `kubectl cp <pod-name>:</path/to/remote/file> </path/to/local/file>`
- access Pod via network
  - `kubectl port-forward <pod-name> 8080:80` (opens a connection that forwards traffic from local machine port 8080 to remote container on port 80)
- see how cluster is using resources
  - `kubectl top <oject>` (can be `nodes` or `pods`)