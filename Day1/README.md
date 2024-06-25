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

## Docker - High Level Architecture
![openshift](DockerHighLevelArchitecture.png)

## Google Kubernetes - High Level Architecture
![openshift](K8sArchitecture.png)


## Red Hat Openshift - High Level Architecture
![openshift](openshiftArchitecture.png)

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


## Info - Pod Overview
<pre>
- Pod is a group of related containers 
- Pod is the smallest unit that can be deployed within Kubernetes/Openshift
- Pod is a resource stored and managed within etcd database by API Server
- Pod never runs anywhere,only  the container within them runs in worker or master nodes
- One Pod can container any number of containers
- Best practice is, 
  - only one main application should run per Pod
  - one Pod must represent a single application or a single application component ( microservice, webserver, app server, db server, etc.,)
- when we deploy our applications into Kubernetes/Openshift, they run inside a container which is part of a Pod
- In case of docker, every running container gets an IP address, but in Kubernetes/Openshift IP address is assigned only on the Pod level not on the container level
</pre>

Creating a pod with plain docker
```
docker run -d --name nginx_pause --hostname nginx gcr.io/google_containers/pause:latest
docker run -d --name nginx --network=container:nginx_pause nginx:latest
docker ps
docker inspect -f {{.NetworkSettings.IPAddress}} nginx_pause
docker exec -it nginx sh
hostname -i
exit
```
In the above, both the nginx_pause container and the nginx containers share the IP address.

Expected output
![pod](pod1.png)
![pod](pod2.png)


## Info - Pod and container states
You may refer the official documention here
<pre>
https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/
</pre>

## Info - What is ReplicaSet ?
<pre>
- Let's say 1000s of users are trying a access a web hosted in a single Pod
- a single Pod instance won't be able to server 1000s of users, hence we may need to add more instances of the Pod
- to create and manage multiple Pod instances of a single application, Kubernetes/Openshift supports something called ReplicaSet
- ReplicaSet is a resource which is stored in etcd database
  - the container image that must be used to create containers under the Pod
  - it has desiredCount - tells how many Pod instances the user expects to run in Openshift
  - it has currentPod - tells how many Pod instances are actually running in the openshift 
  - it has availabe/ready Pod - tells how many Pod are in ready state to server end-users
- ReplicaSet is created by Deployment Controller
- ReplicaSet is accepted as an input by the ReplicaSet Controller
- ReplicaSet Controller creates so many Pods as mentioned in the desiredCount of the ReplicaSet
- ReplicaSet Controller is also reponsible for scale up/down
</pre>

## Info - What is Deployment?
<pre>
- stateless application are deployed as Deployment into Kubernetes/Openshift
- When we run the command 'oc create deployment nginx --image=bitnami/nginx --replicas=3' it creates the following in the openshift cluster
  - a Deployment resource
    - a replicaset resource
      - Pod 1
        - pause container ( running in worker-1 node )
        - nginx container ( running in worker-1 node )
      - Pod 2
        - pause container ( running in worker-2 node )
        - nginx container ( running in worker-2 node )
      - Pod 3
        - pause container ( running in master-2 node )
        - nginx container ( running in master-2 node )
</pre>

## Info - openshift project
<pre>
- Kubernetes supports something called namespace
- Kubernetes/openshift are generally shared by many teams within the organization
- Using namespace we can segregate the applications deployment by one team from the other teams
- Openshift has introduced a new feature called projects, which is based on namespace
- Using Openshifit projects we can give access only the team members while denying access to other users
</pre>

## Info - What is an Openshift Operator?
<pre>
- Kubernetes/Openshift allows adding new functionalities by creating custom resources
- Custom Resources can be added to Kubernetes/Openshift by creating Custom Resource Definitions (CRD)
- In order to manage our Custom Resources, we also need to add Custom Controllers
- As we know, each Controller manages one type of Resource, hence the already existing controllers will have no knowledge about our Custom Resources, hence we need to provide our own Controller
- The combination of Custom Resources and Custom Controller is referred as Operators
- This is supported in both Kubernetes and Openshift
- Example
  - We installed Metallb operator to support LoadBalancer service in bare-metal openshift cluster
  - The metallb operator would have installed many new resources to our cluster
  - The metallb operator would have also installed controllers to manage the custom metallb resources
</pre>

## Lab - Listing the Openshift nodes
```
oc get nodes
kubectl get nodes

kubectl get nodes -o wide
oc get nodes -o wide
```

Expected output
![openshift](nodes1.png)
![openshift](nodes2.png)

## Lab - Describe node to find more details about node
```
oc get nodes
oc describe node//master-1.ocp4.tektutor.org.labs
```

Expected output
![openshift](nodes3.png)
![openshift](nodes4.png)

## Lab - Using explain command to know api details and definition of any resource in Openshift/Kubernetes
```
oc explain node
oc explain deployment
oc explain replicaset
oc explain pod
```

Expected output
![openshift](nodes5.png)
![openshift](nodes6.png)


## Lab - Listing yaml definition of a specific node
```
oc get node/master-1.ocp4.tektutor.org.labs -o yaml
```

Expected output
![openshift](nodes7.png)
![openshift](nodes8.png)

## Lab - Listing all project namespaces in openshift
```
oc get projects
oc get namespaces

oc get project
oc get namespace
```

Expected output
![openshift](project1.png)
![openshift](project2.png)


## Lab - Create a new project in your name
```
oc new-project jegan
```

Expected output
![openshift](project3.png)

## Lab - Finding the currently active project
```
oc project
```

Expected output
![openshift](project4.png)

## Lab - Switching between projects
```
oc project default
oc project jegan
```

Expected output
![openshift](project4.png)

## Lab - Create your first deployment in imperative style
```
oc project jegan
oc project
oc create deployment nginx --image=nginx:latest --replicas=3
```

Listing the deployments in the active project
```
oc get deployments
oc get deployment
oc get deploy
```

Listing the replicasets in the active project
```
oc get replicasets
oc get replicaset
oc get rs
```

Listing the pods in the active project
```
oc get pods
oc get pod
oc get po
```

Expected output
![openshift](output1.png)
![openshift](output2.png)

## Lab - Troubleshooting the Pod CrashLoopBackoff issue
Let's try to understand why the Pod is crashing
```
oc logs nginx-56fcf95486-85bzw
```

Expected output
![openshift](log1.png)

In Kubernetes, the Pod containers can run with/without admin privileges.  Hence, the Kubernetes cluster will not enforce best practices.

In Openshift, the openshift nodes are installed with Red Hat Enterprise Core OS (RHCOS). The RHCOS operating system enforces the best practices are always followed, when the container images doesn't the best practices it won't let the Pod run due to the violations.

In Openshift regular application is supposed to run with non-adminstrator privileges. Hence, not all folders are writable.  In this the docker image that we used is not prepared keep openshift guidelines in mind.  Hence, not all docker images will work in openshift, but the same docker image will work perfectly fine in Kubernetes.


## Lab - Deleting a deployment from your project
```
oc delete deploy/nginx
oc get deploy,rs,po
```

Expected output
![openshift](output3.png)

## Lab - Creating a nginx deployment using bitnami/nginx container image from Docker Hub Remote Registry
```
oc project jegan
oc create deployment nginx --image=bitnami/nginx:1.18 --replicas=3
oc get deploy,rs,po
oc get po -w
```

Listing the pod with their IP and node where they are deployed
```
oc get po -o wide
```

Expected output
![openshift](output4.png)
![openshift](output5.png)


## Lab - Getting inside the nodes where your application pod is running to understand some internal stuffs
```
oc get nodes
oc get po -o wide
oc debug node/master-3.ocp4.tektutor.org.labs

chroot /host

podman version
crictl version
```

List all container images present in the master 3 node
```
crictl images
```

List all containers running in the master node
```
crictl ps
```

List all containers that belong to a specific pod
```
crictl ps | grep nginx-566b5879cb-cqcsz
```

Expected output
![openshift](output6.png)
![openshift](output7.png)
![openshift](output8.png)
![openshift](output9.png)

## Lab - Getting inside a Pod shell
```
oc get deploy,po
oc rsh deploy/nginx

oc exec -it nginx-566b5879cb-kj8rd sh
hostname
hostname -i
exit
```

Expected output
![openshift](output10.png)
![openshift](output11.png)

## Lab - Developer testing using port-foward ( not recommended in production )

The below command is a blocking command, hence to access the web page you need to open another terminal tab
```
oc get po
oc port-forward nginx-566b5879cb-cqcsz 9090:8080
```
In the above command, port 9090 is open on the local machine, while 8080 is the Pod container port where nginx is listening.  So when we access http://localhost:9090 the call is forwarded to pod container port 8080.


In a different terminal tab, you can try this ( this only works on your local linux machine )
```
curl http://127.0.0.1:9090
curl http://localhost:9090
```

To come out of the port-forward, you need to press Ctrl + C

Expected output
![openshift](output12.png)
![openshift](output13.png)

## Lab - Exposing an application only within the cluster

#### When to use ClusterIP Internal Service?
<pre>
- A clusterip service is an internal service
- this type of service can be used in dev,qa and production
- For instance, you have a mysql database deployment with muliple pods that needs to accessed from some microservice pod, you can expose the mysql as a clusterip service
- Normally for all db deployment, it is exposed as clusterip services as they are only supposed to accessed within openshift cluster not exposed to the end users
</pre>

To create a clusterip service, we need to have a deployment first
```
oc project jegan
oc get deploy,rs, po

oc expose deploy/nginx --type=ClusterIP --port=8080
```

Listing the service
```
oc get services
oc get service
oc get svc
```

Finding more details about the clusterip service
```
oc describe svc/nginx
```

Expected output
![output](output14.png)
![output](output15.png)


Accessing the clusterip service from a Pod that belongs to a different deployment
```
oc create deployment hello --image=tektutor/spring-ms:1.0
oc get po -w
```

Get inside the hello pod shell
```
oc rsh deploy/hello
curl http://nginx:8080
curl http://172.30.226.252:8080
```

Expected output
![openshift](output16.png)
![openshift](output17.png)

## Info - What is service discovery in Kubernetes/Openshift?
<pre>
- accessing a service by its name
- In Kubernetes/Openshift, we have DNS Pods, whose job is to translate the service name into its respective service IP address
- In each pod, there will be file called /etc/resolv.conf
- When the pod containers are created by kubelet container agent running on every node, it also configures the /etc/resolv.conf with the openshift dns service IP, which helps in resolving the service name to its respective Ip address
- the nameserver IP address we see 172.30.0.10 is the dns-default service IP, which represents a group of dns pods( one pod per node )
</pre>

Finding the dns service IP address
```
oc get svc -n openshift-dns
```

Expected output
![openshift](output18.png)
![openshift](output19.png)
![openshift](output20.png)
![openshift](output21.png)


## Lab - Creating a nodeport external service for nginx deployment in imperative sytle

First, let's delete the existing clusterip internal service
```
oc get svc
oc delete svc/nginx
oc get svc
```

Let's create the nodeport external service
```
oc expose deploy/nginx --type=NodePort --port=8080
oc get svc
oc describe svc/nginx
```

Let's access the nodeport service
```
oc get nodes
curl http://master-1.ocp4.tektutor.org.labs:31504
curl http://master-2.ocp4.tektutor.org.labs:31504
curl http://master-3.ocp4.tektutor.org.labs:31504
curl http://worker-1.ocp4.tektutor.org.labs:31504
curl http://worker-2.ocp4.tektutor.org.labs:31504
```

Expected output
![openshift](output22.png)
![openshift](output23.png)
![openshift](output24.png)
![openshift](output25.png)
![openshift](output26.png)
![openshift](output27.png)

Points to note
<pre>
- Kubernetes & Openshift reserves port 30000-32767 port range for the purpose of nodeport external service
- Hence, when we create nodeport service any one of the ports on the above range will be randomly picked by Kubernetes/Openshift
- the single Nodeport picked by Openshift is opened on the nodes in the openshift cluster
- each nodeport service, it is going to open one port on every node, so the more nodeport services we creates it ends up opening more number of ports in the openshift cluster, this might lead to security issues
- also the way we access the nodeport service is neither developer friendly nor end-user friendly
- In openshift, we could use route to make a service available outside the cluster scope which kind of addreses all the nodeport issues we discussed earlier
</pre>

## Lab - Creating a load balancer external service for nginx deployment in imperative style
We need to delete the existing nodeport external service
```
oc get svc
oc delete svc/nginx
oc get svc
```

Let's create the load balancer service for nginx deployment
```
oc expose deploy/nginx --type=LoadBalancer --port=8080
oc get svc
oc describe svc
```
In the above output, you would have noticed that the external ip is pending.

Points to note
<pre>
- LoadBalancer service  is used in public cloud environments like AWS, GCP, Azure, Digital Ocean, etc.,
- It is not normally used in local bare metal openshift setup like ours
- In case, you wish to use LoadBalancer external service in a bare-metal openshift cluster, we need to install metallb operator as an Openshift Administrator
- Post installing metallb operator, we need to allocate some available IP address range for the purpose of metall b load balancer
- We also need to create a metallb controller instance by creating a metallb instance 
- In Kubernetes/Openshift, there is a pod called kube-proxy which runs in every node
- kube-proxy provides load-balancing functionality for the clusterip and nodeport services
- In case of Load Balancer service, an external balancer with static ip accessible over internet is assigned for the external load balancer instance created in aws/azure/gcp, etc.,
- the metallb operator spins-off a load-balancer locally within the openshift cluster supporting a similar loadbalancing just like the public clouds
</pre>

## Lab - Installing Metallb Operator as a Openshift administrator
For detailed instructions on how to install Metallb operator in Openshift, you may refer my medium blog
<pre>
https://medium.com/tektutor/using-metallb-loadbalancer-with-bare-metal-openshift-onprem-4230944bfa35  
</pre>

![openshift](output28.png)
![openshift](output29.png)
![openshift](output30.png)
