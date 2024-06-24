# Day 1

## Cloning this repository on the RPS Linux machine
```
cd ~
git clone https://github.com/tektutor/openshift-june-2024.git
cd openshift-june-2024
ls
```

## Testing lab environment
```
oc version
kubectl version

oc get nodes
kubectl get nodes

cat ~/openshift.txt
```

Expected output
![day1](test.png)

## What is Hypervisor ?

<pre>
- virtualization technology
- it allows us running multiple os on the same machine
- many OS can be actively running side by side on the same machine
- each Virtual Machine represents one fully function operating system
- we can install windows/linux/mac in the virtual machine
- the Operating System installed inside the Virtual machine is called Guest OS
- the virtualization software is of two types
  1. Type 1 - Bare Metal Hypervisor ( Installed in Servers/Workstations )
  2. Type 2 - Installed in Laptops/Desktops/Workstations
- this is considered heavy-weight virtualization as each Virtual Machine has to allocated with dedicated hardware resources
  - CPU Cores
  - RAM
  - Disk Storage
  - Grapics Card ( Virtual )
  - Network Card ( virtual )
- Examples
  - VMWare vSphere/vCenter - Type 1 Hypervisor ( Bare Metal ) - Commercial Product
  - VMWare Workstation - Type 2 - Linux/Windows - commercial product
  - VMWare Fusion - Type 2 - Mac - commerical product
  - Oracle Virtual Box - Type 2- works in Linux/Windows/Mac and it is Free
  - KVM - Type 2 - works in all Linux Distributions
  - Parallels - Type 2 - works in Mac
  - Hyper-V - Type 2 - works in Windows Server grade OS
</pre>

## Info - Containerization
<pre>
- is an application virtualization technology
- each container represents one application process
- container are not operating sysytem, they don't their own OS Kernel
- each container either represents one fully function application or a application component
  - application component ( database server, web server, application server, etc., )
  - application - CRM, etc.,
    - an application might requires more than one container in some cases
- light-weight virtualization technology
  - containers doesn't require dedicated hardwares as they use the hardware resources available on the host operating system
- there are some similaries between containers and virtual machines
  - just like Virtual machines acquire their own IP, containers also get their own IP usually (Private IPs)
  - just like Virtual machines has a Network Card, containers also has a network card
  - just like Virtual machines has their own Network stack, containers also has their own network stack
  - containers has a file system just like virtual machine
  - containers has own port range ( 0 - 65535 ) just like virtual machines
- containers will never be able to replace virtual machine as virtual machine runs an Operating System, while container runs a single application
</pre>

## Info - Is it possible to run multiple application within a container?
<pre>
- Yes, it is possible to run multiple applications in a container
- How docker or podman let's you run multiple applications inside a container is, the main container will run a utility called supervisord which will spin separate process to run the applications and the supervisord monitors the health and status of the child processes, which is a overhead, hence though it works it is not a recommnended best practice
- recommendation is, one application per container
</pre>  

## Info - Container Runtime Overview
<pre>
- low-level software to manage container images and containers
- not user-friendly
- generally not used by end-users
- examples
  - CRI-O Container Runtime
  - runC container runtime
</pre>

## Info - Container Engine Overview
<pre>
- high-level software used to manage containers and images
- very user-friendly
- without knowing low-level container related kernel knowledge we can easily manage containers
- internally container engines depend on Container Runtime to manage containers and images
- Examples
  - Docker is a Container Engine which depends on containerd which in turn depends on runC container runtime
  - Podman is a Container Engine which depends on CRI-O Container runtime
</pre>

## Info - Docker Overview
<pre>
- is developed in Golang by Docker Inc organzation
- comes in 2 flavours
  1. Community Edition - Docker CE ( Free )
  2. Enterprise Edition - Docker EE ( Paid )
- follow Client/Server Architecture
- in most cases, when we create containers they provide us root access irrespective of whether you are administrator or not
- Docker supports rootless containers
</pre>

## Info - Podman Overview
<pre>
- is a container engine which internally depends on CRI-O Container Runtime
- Red Hat Openshift supported Docker(run-C) until v3.11
- Red Hat Openshift supports only CRI-O container runtime and Podman within Red Hat Openshift
- Red Hat Openshift v4.x onwards Docker and other container runtime support is removed
</pre>

## Info - Container Orchestration Platforms Overview
<pre>
- though containerized application workloads can be managed manually, in real-world application companies don't manage containers directly/manually
- generally every organization uses Container Orchestration Platforms to manage their containerized application workloads
- Examples
  - Docker SWARM
  - Google Kubernetes
  - Red Hat Openshift
  - AWS - Kubernetes Managed Service called EKS (Elastic Kubernetes Service)
  - Azure - Kubernetes Managed Service called AKS (Azure Kuberentes Service )
  - AWS - Red Hat Managed Openshift service called ROSA
  - Azure - Red Hat Managed Openshift service called ARO
</pre>

#### Docker SwARM
<pre>
- Docker SWARM is Docker Inc native Container Orchestration Platform
- it supports managing only Docker containerized application workloads
- it is light weight setup, hence we can install this in a laptop with even a basic configuration
- it is easy to install, learn
- it is not production-grade, generally used for learning purpose, can be used in dev/qa environment
</pre>


#### Google Kubernetes
<pre>
- developed by Google in Golang
- robust container orchestration platform
- supports many different types of container runtimes including runc, containerd, CRI-O, etc.,
- it also supports adding additional functionality by adding your own Custom Resource and custom controllers
- to extend Kubernetes, we can develop an Kubernetes Operator and add new functionalities
- Operators => is a combination of many Custom Resource + Custom Controllers
- is opensource
- supports command-line interface only
- time tested and robust can be used in Production
- Kubernetes Dashboard ( webconsole - it is basic - it poses some security issues, so the first thing administrators does is to disable this )
</pre>

#### Red Hat Openshift
<pre>
- is developed on top of Google Kubernetes
- Red Hat developed many operators and extended Kubernetes to support many practical features required by IT industry
- Red Hat supports 
  - User Management ( Role Based Access Control - RBAC )
  - Comes with Internal Container Registry
  - Comes with S2I
    - We can deploy application from source code grabbed from Version control softwares ( Not supported in K8s )
    - We build and deploy application from within Openshift
  - CI/CD Platform
  - Supports Virtualization
  - Routes - allows us expose our application with a friendly public url
</pre>

## Red Hat Openshift - Control Plane Components
<pre>
- API Server (Pod)
- etcd key-value data-store (Pod)
- controller managers (Pod)
- scheduler (Pod)
</pre>

#### API Server
<pre>
- support REST APIs for every feature supported in Kubernetes/Openshift
- the entire status and application status is stored in the etcd database by the API Server
- no components in Openshift talk to each other directly
- API Server is the only componenent which can udpate the etcd database
- all openshift components they only communicate with API Server via REST calls
- any time the API Server updates the etcd database it will send a broadcasting event
  - Examples
    - new deployment created
    - new replicaset created
    - new pod created
    - scaled up
    - scaled down
    - deployment updated
    - deployment delete
</pre>

#### etcd
<pre>
- key-value database
- this stores the application and cluster status
- if we backup the etcd, it is very easy to restore the same cluster else where
- opensource, independent project which can be used outside the scope of kubernetes/openshift
</pre>

#### controller managers
<pre>
- a group of many controller which provides monitoring
- each controller manages one type of resource in Kubernetes/Openshift
- Deployment Controller manages Deployment resource
- ReplicaSet Controller manages ReplicaSet resource
- Examples
  - Deployment Controller
  - ReplicaSet controller
  - StatefulSet controller
  - Job Controller
  - CronJob Controller
  - Endpoint Controller
  - DaemonSet Controller
</pre>

#### scheduler
<pre>
- this components recommends on which node a newly created Pod can be deployed  
- scheduler can't deploy a pod directly, hence it sends its scheduling recommendations to API Server via REST calls
</pre>
