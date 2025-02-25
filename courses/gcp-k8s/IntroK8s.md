# Introduction to Containers and Kubernetes

## Introduction to Containers
We used to build apps on individual servers, where each machine was dedicated to a single application / use-case.

This, as you may imagine, was not scalable, not cost-effective, and not portable.

Virtual Machines (VMs) were created to solve this problem - they allowed us to run multiple virutal machines on a single machine.

VMs are great, but running multiple applications within a single VM creates several issues:
- Resource contention
- Lack of isolation
- Dependency conflicts

Containers are a solution to this problem - we implement abstraction at the application level. We don't need to virtualize the entire machine or the OS, just the user space.

### Quiz
**Which of these problems are containers intended to solve? Mark all that are correct (3 correct answers).**
- [x] Applications need a way to isolate their dependencies from one another.
- [x] Packaging applications in virtual machines can be wasteful.
- [ ] Large monolithic applications that need to be run in the cloud.
- [x] It's difficult to troubleshoot applications when they work on a developer's laptop but fail in production.

## Container and Container Images
An application and its dependencies are packaged into a container image. A container is an instance of an image. Docker is the most popular image and container runtime.

While Docker is amazing, it does not have the same capability to scale as Kubernetes.

Docker images are split into layers, of which each layer is a read-only layer. The layers are stacked on top of each other, and when running a container, a thing Read / Write layer is added to the top layer, also known as the Container layer. These days, typically, application packages are built using multi-stage builds, where one container builds the final executable image, and a separate container receives only what is needed to run the application.

Example:

```Dockerfile
# ---- Build Stage ----
FROM golang:1.21 AS builder
WORKDIR /app
COPY . .
RUN go build -o myapp

# ---- Runtime Stage ----
FROM alpine:latest
COPY --from=builder /app/myapp /usr/local/bin/myapp
CMD ["myapp"]
```

This way:

- Smaller Images: Final image has only what’s needed to run (no compilers, dev tools, or source code).
- Better Security: Fewer packages = fewer vulnerabilities in production.
- Clean Separation: Build tools (e.g., gcc, npm) stay in temporary stages, not the final image.
- Efficiency: Faster builds (cached stages) and smaller layers.

### Quiz
Why do Linux containers use union file systems?
- [x] To efficiently encapsulate applications and their dependencies into a set of clean, minimal layers
- [ ] To give a container its own virtual memory address space
- [ ] To control an application's ability to see parts of the directory tree and IP addresses
- [ ] To control an application's maximum consumption of CPU time and memory

What is significant about the topmost layer in a container? Choose all that are true (2 correct answers).
- [x] An application running in a container can only modify the topmost layer.
- [ ] Reading from or writing to the topmost layer requires special software libraries.
- [ ] Reading from or writing to the topmost layer requires special privileges.
- [x] The topmost layer's contents are ephemeral. When the container is deleted the contents will be lost.

## Introduction to Kubernetes
How do you manage containers at scale? What can you do to better manage your container infrastructure? Kubernetes is an open-sourced container orchestration tool that helps you manage your container infrastructure at scale.

It automates the deployment, scaling, load balancing, logging, monitoring, and other management features of containerized applications. These are featrues that are characteristic of a typical platform as service solutions

Kubernetes also facilitates the features of an infrastructure as a service, such as allowing a wide range of user preferences and configuration flexibility.

Most importantly, Kubernetes supports declarative configuration, which allows you to describe the state of your infrastructure that you desire, rather than a set of commands to achieve that state. Kubernetes' job is to make the deployed system maintain the desired state in the face of changing conditions.

It is possible to administer imperative configuration commands to Kubernetes, ie, during outages or specific events. However, the recommended approach is to use declarative configuration.

Features to highlight:
1. Supports both stateful and stateless applications
   1. It supports stateless applications such as Nginx or Apache webservers, and stateful applications, where user and session data can be stored persistently. It also supports batched jobs and daemon tasks.
2. Autoscaling
   1. Kubernetes can automatically scale in and out containerized applications based on resource utilization.
3. Resource limits
   1. You can specify resource requests levels and resource limits for your workloads and Kubernetes will obey them.

### Quiz
**When you use Kubernetes, you describe the desired state you want, and Kubernetes's job is to make the deployed system conform to your desired state and to keep it there in spite of failures. What is the name for this management approach?**
- [ ] Virtualization
- [ ] Containerization
- [ ] Imperative configuration
- [x] Declarative configuration

**What is a stateful application?**
- [ ] An application that is not containerized
- [ ] A web front end
- [x] An application that requires user and session data to be stored persistently

## Introduction to Google Kubernetes Engine
Google Kubernetes Engine (GKE) is a good way to let Google Cloud manage your Kubernetes clusters and other related infrastructure.

> [!NOTE]
> GKE uses a container-optimized OS, which will be discussed later in this course.
GKE can help you deploy, manage, and scale Kubernetes environments for your containerized applications, some notable features are noted below:

### Autopilot
GKE Autopilot is a mode of operation for GKE in which GCP manages your cluster configuration, including your nodes, scaling, security, and other pre-configured settings.

### Auto Repair
The VMs that host your containers in a GKE cluster are called nodes. If you enable GKE's auto-repair feature, the service will repair unhealthy nodes for you. It does this by making periodic health checks on each node of the cluster. If a node is determined to be unhealth and requires repair, GKE will drain the node (gracefully shut down the node) and recreate the node.

### Auto Upgrade
When you use GKE, you start by directing the service to create a Kubernetes system, also known as a cluster. If you enable auto-upgrade, GKE will automatically upgrade the cluster to the latest stable version of Kubernetes.

### Auto Scaling
Just as Kubernetes supports scaling of its workloads, GKE also supports scaling of your clusters.

### CI/CD integrations
GKE seamless integrates with GCP's Cloud Build and Container Registry, enabling automated builds and deployments.

### Quiz

**What is the relationship between Kubernetes and Google Kubernetes Engine?**
- [x] Google Kubernetes Engine is Kubernetes as a managed service.
- [ ] Kubernetes and Google Kubernetes Engine are two names for the same thing.
- [ ] Google Kubernetes Engine is a closed-source variant of Kubernetes.

**Which of the following supports scaling a Kubernetes cluster as a whole?**
- [x] Google Kubernetes Engine
- [ ] Compute Engine
- [ ] Kubernetes

**What is the name for the computers in a Kubernetes cluster that can run your workloads?**
- [ ] Container images
- [ ] Control Planes
- [x] Nodes
- [ ] Containers

## Compute Options Detail
In a previous module, we outlined that there are several options for running compute workloads in GCP. These options are:
- Compute Engine
- App Engine
- GKE
- Cloud Run
- Cloud Functions

### Compute Engine
Compute Engine revolves around VMs that run on GCP.

Fully customizable VMs:
- You can choose from pre-defined VM configurations
- Or create your own custom configuration to match your performance and cost requirements.

Persistent storage:
- Virtual Machines need block storage, and Compute Engine offers persistent disks and SSDs.
- Persistent disks offer network storage that can scale up to 64TB, and you can easily take snapshots for backup and mobility
- SSDs have higher throughput and lower latency, but are more expensive

Load balancing and auto-scaling:
- You can place your Compute Enginer workloads behind global load balancers that support auto-scaling.
- Compute Engine offers a feature called managed instance groups, which allows us to define resources that are automatically deployed to meet demand.

Per-second billing:
- GCP enables fine-grain control of costs of Compute Engine resources by providing per second billing. This granularity helps to reduce costs when deploying compute resources for short periods of time, such as batch processing jobs.
- Compute Engine offers pre-emptible VMs, which provides significantly cheaper pricing for your workloads that can be interrupted safely.

**So, why do people choose Compute Engine?**
- Complete control over infrastructure
- Customization of OS and software
- Easy migration between on-premises and GCP (since it's just a VM underneath)
- Flexible - it'll always work if other options don't quite fit your needs

### App Engine
App Engine has a completely different orientation from Compute Engine. App Engine is a fully-managed application platform, which means zero management and configuration of the underlying infrastructure - GCP does all of that for you.

Fully code-first platform:
- You write code, and GCP handles the rest
- App Engine supports popular languages like Java, Node, Python, Go, etc.

Supports integrated monitoring, logging, and diagnostics:
- Stackdriver is GCP's operations suite

Supports Version Control and Traffic Splitting:
- You have access to version control, canary testing, and rollbacks

**So, why do people choose App Engine?**
- Focus on development, not deployment
- Easy way to deploy and scale websites, mobile backends, and APIs

### GKE
GKE is a fully managed Kubernetes service that allows you to run your containerized workloads on GCP, automating deployment, scaling, load balancing, logging, monitoring, and so on.

Management:
- GKE supports cluster scaling, persistent disks, automated upgrades, and auto node repairs

Integration:
- Built-in integration with Cloud Build, Container Registry, Stackdriver Monitoring, and Stackdriver Logging

Portability:
- Existing workloads running on-premise can be easily moved to GCP, since Kubernetes has no vendor lock-in.

**So, why do people choose GKE?**
- Containerized workloads
- Cloud-native distributed systems
- Hybrid applications

### Cloud Run
Cloud Run is a managed compute platform that enables you to run stateless contaienrs via web requests or Cloud Pub / Sub events.

Stateless containers:
- Cloudrun is stateless and serverless, designed to abstract away infrastructure management so you can focus on building your application.
- Built on Knative, an open source Kubernetes-based platform, and builds, deploys, and manages modern serverless workloads
- Cloud Run lets you run containers fully-managed or in your own GKE cluster

Abstracts away infrastructure management:
- Cloud Run enables you to run request or event-driven workloads without having to worry about servers
- Abstracts away all infrastructure management, including provisioning, configuring, and managing servers

Auto-scaling:
- Automatically scales depending on traffic
- Cloud Run only charges you for resources that you use

**So, why do people choose Cloud Run?**
- Deploy stateless containers that listen for requests or events
- Build applications in any language using any framework and tools

### Cloud Functions
Cloud Functions is an event-driven serverless compute service for simple, single-purpose functions that are attached to events.

Event-driven, serveless compute:
- Upload code, and GCP will deploy the appropriate compute capacity to run that code
- Your code will only run based on some event that you configure

Auto-scaling:
- Servers are scaled based on number of requests, and are highly available and fault tolerant

Cost-effective:
- Only charged for the compute time your code uses
- Perpetual free tier

**So, why do people choose Cloud Functions?**
- Supporting microservice architecture
- Serverless application backends
- Intelligent applications (Chatbots)

### Which do we choose?
It all depends on where you're coming from:

If you're running applications on a physical server, the path of least resistance is Compute Engine.

What if you're running applications in long-lived VMs for which each VM is managed and maintained? Compute Engine is also a good choice here.

What if you don't want to think about infrastructure at all? Then, App Engine and Cloud Functions are great choices.

Containerization is popular, and both App Engine and Compute Engine support images and containers. However, what if you wanted more control of your containerized workloads than what App Engine offers and denser packing than what Compute Engine offers?

Then you'll want to use GKE!

## Summary Quiz
**Google Compute Engine provides fine-grained control of costs. Which Compute Engine features provide this level of control? (2 correct responses)**
- [ ] Managed instance groups
- [ ] Billing budgets and alerts
- [ ] Autoscaling groups
- [x] Fully customizable virtual machines
- [x] Per-second billing

**You are deploying a containerized application, and you want maximum control over how containers are configured and deployed. You want to avoid the operational management overhead of managing a full container cluster environment yourself. Which Google Cloud compute solution should you choose?**
- [x] Google Kubernetes Engine
- [ ] Cloud Functions
- [ ] Compute Engine
- [ ] App Engine

**You are classifying a number of your applications into workload types. Select the stateful applications in this list of applications. Choose all responses that are correct (2 correct responses).**
- [ ] Image recognition application that identifies product defects from images.
- [x] A shopping application that saves user shopping cart data between sessions.
- [x] A gaming application that keeps track of user state persistently.
- [ ] Web server front end for your inventory system.

**You are choosing a technology for deploying applications, and you want to deliver them in lightweight, standalone, resource-efficient, portable packages. Which choice best meets those goals?**
- [x] Containers
- [ ] Executable files
- [ ] Virtual Machines
- [ ] Hypervisors
