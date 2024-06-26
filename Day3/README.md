# Day 3

## Lab - Deploying an application using source strategy from command line
```
oc new-app registry.access.redhat.com/ubi8/openjdk-11~https://github.com/tektutor/openshift-june-2024.git --context-dir=Day2/hello-microservice --strategy=source
```

To check the build log
```
oc logs -f buildconfig/openshift-june-2024
```

We need manually create route
```
oc expose svc/openshift-june-2024
```

You may access the route url to check the output from the microservice
```
curl http://http://openshift-june-2024-jegan.apps.ocp4.tektutor.org.labs
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/96e93ddb-e675-49c2-ba2e-e0e2220a203f)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/7cd5cccc-832d-421e-8421-45c447168098)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/04747539-6f13-4f31-8660-d6996abbcfa5)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/fc06f716-fe60-4f29-82e4-dcefe901b910)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/66c7b691-dd23-45da-872d-909ac73e4714)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/e107fbac-8e3b-49d0-a7a6-e8982eceb60a)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/e4e562de-b97e-4df9-8474-00f4bb099238)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/6606a937-fda4-4a12-b30d-d07bbbadf8cc)

## Lab - Deploying application using existing docker image
```
oc new-project jegan
oc new-app --name=nginx --image=bitnami/nginx:latest
oc expose svc/nginx
oc get route
curl http://nginx-jegan.apps.ocp4.tektutor.org.labs
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/5f153733-6305-412e-a2ef-3c7497287139)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/42bd35ba-ad68-46d6-b17a-85a5149f0a0c)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/4eba8315-9e2c-4414-a29c-5ec79547b953)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/3abe9e1e-fdd2-4e8b-b1ca-136cd72679d7)

## Lab - Finding list of red hat container image options available for a programming language stack
```
oc new-app --search java
oc new-app --search python
oc new-app --search dotnet
oc new-app --search ruby
oc new-app --search php
oc new-app --search nodejs
oc new-app --search angular
oc new-app --search react
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/19ff301c-3bdc-44f6-be3a-9a6a7bee50eb)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/f465d995-6a13-4f38-9da3-c0f0f654d037)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/f1fec2b5-c76a-41d9-ada4-04b6197d907a)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/aed2c6de-3fa8-4251-85bc-a84f3f3bf434)

## Lab - Deploying nginx in declarative style
```
oc delete project jegan
oc new-project jegan
oc create deployment nginx --image=bitnami/nginx:1.18 --replicas=3 -o yaml --dry-run=client
oc create deployment nginx --image=bitnami/nginx:1.18 --replicas=3 -o yaml --dry-run=client > nginx-deploy.yml
cat nginx-deploy.yml

oc create -f nginx-deploy.yml
oc get deploy,rs,po
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/6c6f51f5-44bc-4d9f-845b-e41c6c483811)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/d9052917-3363-47ea-a969-d02a445729bc)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/28146eb6-4604-482e-9616-b5543bd499d0)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/1afd2f8a-f498-492b-9927-76d59652d341)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/700ca36f-5f09-4f6b-bd8a-d160c071b526)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/61bcd2f2-a4e2-4f58-9c7d-b2a6d2238de4)

## Lab - Scale up/down deployment in declarative style

You may edit the nginx-deploy.yml and update the replicas from 3 to 5, save it and apply the changes as shown below
```
cat nginx-deploy.yml
oc apply -f nginx-deploy.yml
oc get po
oc get po -w
oc get po
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/1fac5af2-4535-4c61-96ad-77beb85fc90a)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/9a2c3e23-dcc6-496d-98a1-b91d4b740977)

## Lab - Deploying replicaset in declarative style
```
oc get rs
oc get rs -o yaml
oc get rs -o yaml > nginx-rs.yml
vim nginx-rs.yml
cat nginx-rs.yml
oc create -f nginx-rs.yml
oc get deploy
oc get rs,po
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/fce0098f-4ace-4a66-bda8-626065964ae6)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/7a355224-e255-4c6b-812b-c7fd1631b668)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/5e074c44-86ce-484c-aa32-31353a912071)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/8e23db8d-89e3-4f2e-85ac-4301eaf1776c)

#### Points to note
<pre>
- Though creating a replicaset without deployment is possible, it is not a best practice
- Without deployment, we won't get self-healing for replicaset, hence if the replicaset is deleted all the pods will be deleted
- the other reason, why this isn't a best practice is, we won't be able to perform rolling update as there is no deployment. Only scale up/down is possible
- Hence, always consider using deployment
</pre>  

## Lab - When to use oc create vs oc apply?
<pre>
- oc create should be used when the deployment doesn't exist in the cluster already 
- once the deployment is created in the cluster, we can't use create anymore, we can only use apply
- apply will update the delta changes done in the yml on the existing deployment resource in the openshift cluster
</pre>

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/802244b9-53a0-47d3-add3-f70271981c6a)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/5bef5385-7e3b-4679-b130-db2f8d9d5050)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/35291af0-dc74-4fe4-ac24-cf2c82e15080)

## Lab - Creating a Pod in declarative style
Delete the existing replicaset and associated pods before proceeding
```
cd ~/openshift-june-2024
git pull
cd Day3/declarative-manifest-scripts
oc delete -f nginx-rs.yml
```

Let's create the pod in declarative style
```
cat my-pod.yml
oc create -f my-pod.yml
oc get po
oc get po -w
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/18876cb4-04df-4c32-b789-41fd12cfeccf)

## Lab - Creating clusterip internal service in declarative style

First let's delete any existing deployments, replicasets, pod in declarative style
```
cd ~/openshift-june-2024
git pull
cd Day3/declarative-manifest-scripts
oc delete -f pod.yml
oc delete -f nginx-rs.yml
oc delete -f nginx-deploy.yml
oc get all
```

Let's now create then nginx deployment in declarative style and create a clusterip internal service also in declarative style
```
cd ~/openshift-june-2024
git pull
cd Day3/declarative-manifest-scripts

oc create -f nginx-deploy.yml
oc expose deploy/nginx --type=ClusterIP --port=8080 -o yaml --dry-run=client
oc expose deploy/nginx --type=ClusterIP --port=8080 -o yaml --dry-run=client > nginx-clusterip-svc.yml
oc create -f nginx-clusterip-svc.yml
oc get svc
oc get describe svc/nginx
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/8afb1344-5a88-4856-b986-e613f54578d1)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/91bb435c-9db9-4404-b46b-943a219efb08)


## Lab - Creating a nodeport external service in declarative style

First we need to delete the nginx clusterip service
```
cd ~/openshift-june-2024
git pull
cd Day3/declarative-manifest-scripts
oc delete -f nginx-clusterip-svc.yml
oc get svc
```

Let's create the nodeport service for existing nginx deployment
```
oc get deploy
oc expose deploy/nginx --type=NodePort --port=8080 -o yaml --dry-run=client
oc expose deploy/nginx --type=NodePort --port=8080 -o yaml --dry-run=client > nginx-nodeport-svc.yml
oc create -f nginx-nodeport-svc.yml
oc get svc
oc describe svc/nginx
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/f86dd9b4-200d-475c-9820-4e82c5211a6e)

## Lab - Creating a loadbalancer service for existing nginx deployment in declararive style

First we need to delete existing nodeport service
```
cd ~/openshift-june-2024
git pull
cd Day3/declarative-manifest-scripts
oc delete -f nginx-nodeport-svc.yml
oc get svc
```

Let's create the loadbalancer service for nginx deployment in declarative style 
```
cd ~/openshift-june-2024
git pull
cd Day3/declarative-manifest-scripts
oc expose deploy/nginx --port=8080 --type=LoadBalancer -o yaml --dry-run=client
oc expose deploy/nginx --port=8080 --type=LoadBalancer -o yaml --dry-run=client > nginx-lb-svc.yml
oc create -f nginx-lb-svc.yml
oc get svc
curl http://192.168.122.90:8080
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/31522aef-e3f7-4752-b601-45db57700a87)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/4b9eb459-ed5b-4e16-8c6e-1cff20e2bc9a)

## Lab - Rolling update in declarative style
```
oc get deploy
oc get po
oc get po/nginx-566b5879cb-pmhzb -o yaml | grep image
cat nginx-deploy.yml| grep image
oc apply -f nginx-deploy.yml
oc get rs
oc rollout status deploy/nginx
oc rollout history deploy/nginx
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/8968a551-f26d-4abb-8793-d0faaa2953b5)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/d2312f98-09fb-4542-9a48-ddaaff2967dd)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/7bdbff7d-b682-4f85-a5e5-0d8551ad2c69)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/9b58fb8c-e1b6-4daf-a799-3c0b11762248)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/aa2b11ab-ae73-49ee-9ff0-3966060137ac)

## Lab - Declaratively deploying application from Openshift webconsole
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/d2312f98-09fb-4542-9a48-ddaaff2967dd)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/07bfeb30-7c0f-4695-91f3-0e00ea36c06d)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/7c577377-04ee-4ae3-bab5-510148171a72)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/2ed057dd-9efa-4472-9f92-7ad01c35b0a5)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/754a988c-5019-4d1e-aa75-debc7d9758ce)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/8c036542-7e14-4f3f-8035-16ca9fd2f65b)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/85519f5d-87a2-494c-91a8-3db6e3bb48db)
