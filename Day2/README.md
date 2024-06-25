# Day 2

## Info - SOLID Design Principle
<pre>
S - Single Responsibility Principle (SRP)
O - Open Closed Principle (OCP)
L - Liskov Substitution Principle (LSP)
I - Interface Seggregation
D - Dependency Injection or Dependency Inversion or Inversion of Control(IOC)
</pre>

#### Single Reponsibility Principle 
<pre>
- One component should do just one thing
- One component should represent a single entity/object
</pre>

## Info - ReplicationController
<pre>
- In older versions of Kubernetes, stateless application were deployed as ReplicationController
- ReplicationController supports
  - Rolling update
  - Scale up/down
- ReplicationController doesn't support declaratively performing scale up/down
- ReplicationController doesn't support declaratively performing rolling update
- For these reasons, latest versions of Kubernetes, they introducted Deployment & ReplicaSet as an alternate to ReplicationController
- Deployment supports rolling update
- ReplicaSet supports scale up/down
- Deployment supports declaratively performing rolling update and scale up/down
- In Openshift, before the Deployment and ReplicaSet was introducted, they wanted to support scale up/down and rolling updte in declarative style, hence they created DeploymentConfig
- DeploymentConfig internally used ReplicationController
- Once the Deployment & ReplicaSet was introduced in Kubernetes, Openshift deprecated the use of DeploymentConfig
- Hence, new application deployment should avoid using DeploymentConfig and ReplicationController.  Instead, we should consider using Deployment & ReplicaSet
</pre>

## Lab - Create a public url using route for deployment
```
oc get deploy
oc expose deploy/hello --port=8080
oc get svc
oc expose svc/hello
oc get route
curl http://hello-jegan.apps.ocp4.tektutor.org.labs
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/ee1fd71d-c191-4905-bd06-a0847d10d594)

#### Points to note
<pre>
- Route is a new feature introduced in OpenShift
- Route is based on Kubernetes Ingress
- Route provides a user-friendly public url to access the application from outside the cluster
- This is a better alternate for Kubernetes Node Port service
</pre>

## Lab - Ingress

In case you haven't already deployed nginx, you need to deploy nginx as shown below
```
oc project jegan
oc create deployment nginx --image=bitnami/nginx:latest --replicas=3
oc expose deploy/nginx --port=8080
oc get svc
oc describe svc/nginx
```

In case you haven't already deployed hello, you need to deploy hello microservice as shown below
```
oc project jegan
oc create deployment hello --image=tektutor/hello:4.0 --replicas=3
oc expose deploy/hello --port=8080
oc get svc
oc describe svc/nginx
```

Now let's create the ingress forwarding rules to the above services based on path /nginx or /hello.
```
cd ~/openshift-june-2024
git pull
cd Day2/ingress
cat ingress.yml
oc apply -f ingress.yml
oc get ingress
oc describe ingress/tektutor
curl http://tektutor.apps.ocp4.tektutor.org.labs/nginx
curl http://tektutor.apps.ocp4.tektutor.org.labs/hello
```

The domain must match with the registered domain in ingress controller
```
oc describe ingresscontroller default -n openshift-ingress-operator | grep Domain
```

#### Points to note
<pre>
- Ingress is a set of forwarding rules
- Ingress rules are picked by Ingress Controller
- There are two commonly used Ingress Controlls
  1. Nginx Ingress Controller
  2. HAProxy Ingress Controller
- For Ingress to work we need 3 major components within openshift/kubernetes cluster
  1. Ingress Forwarding rules (user-defined)
  2. Ingress Controller
  3. Load balancer
</pre>

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/f73abb86-93cd-4216-8697-8bcac3871809)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/d1a56e1e-f134-4000-85ff-de6dcffbbb1b)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/0a0e955e-01c6-45de-82ba-4d981ca55bc4)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/7f08d052-2874-4119-9a86-90b50cc3fc20)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/e0f936cd-a587-4a10-b782-78dda61886ca)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/3b789032-2aca-436b-b41c-d7081bb653b6)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/87d0c5ca-b11d-4987-9f0b-71cb2f8bd7d4)

## Info - Subnet
<pre>
- If we take IPV4 IP addresses it is 32 bits(4 bytes)
- It has 4 Octets
  - A.B.C.D
  - A is 1 Byte(8 bits)
  - B is 1 Byte(8 bits)
  - C is 1 Byte(8 bits)
  - D is 1 Byte(8 bits)
- Consider this Subnet - 10.128.0.0/24 ( 256 IP Addresses are supported )
- What is IP Address in the above Subnet
  - 10.128.0.0
  - 10.128.0.1
  - 10.128.0.2 
  - ...
  - 10.128.0.255
- The 24 in 10.128.0.0/24 indicates how many bits from left to right are fixed

- From the subnet 10.244.0.0/16 compute 5 Subnets for master-1, master-2,master-3, worker-1 and worker-2 nodes
  - Master 1 - Subnet ( 10.244.1.0/24 )
  - Master 2 - Subnet ( 10.244.2.0/24 )
  - Master 3 - Subnet ( 10.244.3.0/24 )
  - Worker 1 - Subnet ( 10.244.4.0/24 )
  - Worker 2 - Subnet ( 10.244.5.0/24 )
</pre>

## Info - Private IP
<pre>
- Private IP are accessible only on the same machine   
</pre>


## Lab - Deploying Angular application into openshift using Docker strategy cloning source from GitHub
#### What does the below command do?
<pre>
- The below command will deploy angularjs application into openshift by cloning the source code from GitHub repo
- navigates to Day2/angular/Angular-openshift-example folder
- since we have mentioned docker strategy, it looks for Dockerfile under Day2/angular/Angular-openshift-example folder
- Openshift creates a buildconfig with the Dockerfile, the output of the buildconfig will be a docker image which will get pushed into Openshift internal container registry
- Using the newly build image, it automatically deploys the application and creates a service for the deployment
- We need to manually create a route to access the application from outside the cluster
</pre>
```
oc project jegan

oc new-app --name=angular https://github.com/tektutor/openshift-june-2024.git --context-dir=Day2/angular/Angular-openshift-example --strategy=docker

oc expose svc/angular
oc get buildconfigs
oc logs -f bc/angular
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/01682a92-f5af-4f28-b09c-55220ffb7a26)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/029e0902-cab0-47ac-b4bf-d4ccb7a71fb4)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/fe8c73ba-28b6-426b-87de-c8d2a34424a4)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/9c6764af-16ee-4ba5-a1fc-4dd55e537d07)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/18b2fe4a-aa0a-4943-a14a-888d98759789)
