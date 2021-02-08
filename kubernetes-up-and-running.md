
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