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
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/f1812aca-fc0b-4686-8042-3b1704848a38)

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
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/85519f5d-87a2-494c-91a8-3db6e3bb48db)

## Lab - Deleting a deployment from openshift webconsole
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/b0d43be0-5b64-430b-903f-714ea674cd04)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/b833b52d-9eda-4b07-9510-f17dd5e2e835)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/2bf4915c-7724-418d-86d7-6b4a439b6bf5)

## Lab - Deploying mysql with Persistent Volume

#### Points to note
<pre>
- Persistent Volume is an external storage created by System Administrator
- Persistent Volumes can be provisioned by System Administrators
  - Manually ( By defining Persisten Volumes in a yaml file and apply )
  - Dynamically ( Storage Class )
- Persistent Volumes
  - will have a size in MB/GB
  - Will have AccessModes
  - will have Storage Class (optionally)
- Persitent volumes are available for any applications cluster-wide

- Application that require external storage will have ask for storage by defining PersistentVolumeClaim
  - Claim will have to mention
  - What is the size required?
  - What is the accessMode required?
  - Storage Class ( optionally)
  - PVC is defined by the development
- Openshift Storage Controller will search the cluster for matching PersistentVolume as per the PersistentVolumeClaim requirement, if it finds a match then, it will let the PVC go and bound the PV and use it any application deployment
</pre>


```
cd ~/openshift-june-2024
git pull
cd Day3/persistent-volume
cat mysql-pv.yml
showmount -e
oc apply -f mysql-pv.yml
oc get persistentvolumes
oc get persistentvolume
oc get pv

cat mysql-pvc.yml
oc apply -f mysql-pvc.yml
oc get persistentvolumeclaims
oc get persistentvolumeclaim
oc get pvc

cat mysql-deploy.yml
oc apply -f mysql-deploy.yml
oc get deploy
oc get po -w
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/97bf1215-62b0-4c47-b97a-e75cacad4721)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/3ce513a1-054d-4d4f-875b-36793a90e734)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/02581cef-dda5-4474-9c87-5fdbc4d8e42e)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/3b9af9ab-8d76-43e5-aa04-7a0efd4cc238)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/886ae8fd-c93b-4ea8-904f-8ee6d7dcdc97)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/b4546358-6ff7-4a06-83d0-91044b37b219)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/d1990587-836e-4493-aebf-77a899f352c5)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/0e5971f5-8d63-4370-a6d2-383ab027e289)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/4fc18e90-cd8c-41fb-9b49-4f0ddd729a2a)

## Lab - Deploying wordpress and mariadb multi-pod application in declarative style
![wordpress](wordpress.png)

```
cd ~/openshift-june-2024
git pull
cd Day3/persistent-volume/wordpress
oc apply -f mariadb-pv.yml
oc apply -f mariadb-pvc.yml
oc apply -f mariadb-deploy.yml
oc apply -f mariadb-svc.yml

oc apply -f wordpress-pv.yml
oc apply -f wordpress-pvc.yml
oc apply -f wordpress-deploy.yml
oc apply -f wordpress-svc.yml
oc apply -f wordpress-route.yml

oc get po
oc logs mariadb-5b9895469b-mqhbl
oc logs wordpress-98c9cb676-k7hvk

oc get route
```


Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/ec8f6e48-70dd-4ea8-a4e5-a7528b89977b)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/ebd8a635-5f8e-4897-acfb-c1be6013c20e)

Once you are done with the lab exercise, you may delete the wordpress and mariadb in the reverse order
```
cd ~/openshift-june-2024
git pull
cd Day3/persistent-volume/wordpress
oc delete -f wordpress-route.yml
oc delete -f wordpress-svc.yml
oc delete -f wordpress-deploy.yml
oc delete -f wordpress-pvc.yml
oc delete -f wordpress-pv.yml

oc delete -f mariadb-svc.yml
oc delete -f mariadb-deploy.yml
oc delete -f mariadb-pvc.yml
oc delete -f mariadb-pv.yml
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/4d188969-12c7-4f83-9b81-f8739f7b3899)

## Lab - Wordpress & Mariadb Multi Pod application fetching values from configmap and secret
#### Points to note
<pre>
- configmap can be used to store environment variables, tools path, etc
- configmap stores them as key-value pair
- the information stored in configmap should be non-sensitive in nature
- in case, you wish to store any sensitive information like password, you can use secret for the same
- secret also internally stores the data in the form of key-value pair just like configmap
- secret won't reveal the value stores
- we need to provide the values as a base64 encoded string
</pre>


Make sure the wordpress and mysql deployed earlier are deleted before proceeding
```
cd ~/openshift-june-2024
git pull
cd Day3/persistent-volume/wordpress
oc delete -f wordpress-route.yml
oc delete -f wordpress-svc.yml
oc delete -f wordpress-deploy.yml
oc delete -f wordpress-pvc.yml
oc delete -f wordpress-pv.yml

oc delete -f mariadb-svc.yml
oc delete -f mariadb-deploy.yml
oc delete -f mariadb-pvc.yml
oc delete -f mariadb-pv.yml

cd ../mysql
oc delete -f mysql-deploy.yml
oc delete -f mysql-pvc.yml
oc delete -f mysql-pv.yml
```


Now you may proceed deploying mariadb and wordpress as shown below
```
cd ~/openshift-june-2024
git pull
cd Day3/persistent-volume/wordpress-with-configmap-and-secret
cat wordpress-cm.yml
cat wordpress-secret.yml
cat mariadb-deploy.yml
cat wordpress-deploy.yml

oc apply -f wordpress-cm.yml
oc apply -f wordpress-secret.yml
oc apply -f mariadb-pv.yml
oc apply -f mariadb-pvc.yml
oc apply -f mariadb-deploy.yml
oc apply -f mariadb-svc.yml

oc apply -f wordpress-pv.yml
oc apply -f wordpress-pvc.yml
oc apply -f wordpress-deploy.yml
oc apply -f wordpress-svc.yml
oc apply -f wordpress-route.yml
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/71ac758f-d25f-43ac-98e9-00412b651af1)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/b694883c-85c2-4bac-a22f-147f5f46b1a2)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/6c637551-5a88-4a4f-b085-1f316323d07b)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/1fb63152-a3c0-4b12-9ceb-c72bae6f938b)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/837a3e68-963b-40a7-b100-4c829ac369ff)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/2a2ce229-cc6a-4c0e-93df-55433d27388b)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/5eaeccfc-62c3-467c-8441-f84c0529a78c)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/be4edebc-0617-4915-b416-3fd8c906283e)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/4d534b94-3090-4f20-9210-db7891580036)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/5adf498a-4d30-48ed-bf52-6d9aae755617)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/3156bcba-d264-4ba1-85d6-5e7cd33734be)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/244cd520-3578-41c5-8613-e193a0d74463)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/47dd48b0-77b8-4914-a598-a43f4cfd37a3)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/37cbc699-4c08-484a-804d-1c3adeafca1f)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/38f58f4a-9534-4564-830b-7fd4887364bc)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/17ae51ef-4dbe-4692-8962-ba03f92e560b)

## Lab - Deploying redis with persistent volume
```
cd ~/openshift-june-2024
git pull
cd Day3/persistent-volume/redis
showmount -e
cat redis-pv.yml
cat redis-pvc.yml
cat redis-deploy.yml

oc apply -f redis-pv.yml
oc apply -f redis-pvc.yml
oc apply -f redis-deploy.yml

oc get po
oc logs redis-c7bf8bd5-xf2ng
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/27102492-2cc0-40ef-924e-7ea4e1f5a166)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/d2319149-fcb0-4839-8b0d-843e63d27c67)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/423700be-fba8-427d-a3da-7fad59462282)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/25185f5d-3d5f-4419-b9fb-eb1c13fd2a81)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/31fbef8c-6a0c-4c83-a682-67cdc7a9443d)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/3df214fc-6708-4254-a7a6-b89586686eda)


## Lab - Deploying mongodb with Persistent volume
```
cd ~/openshift-june-2024
git pull
cd Day3/persistent-volume/mongodb
cat mongodb-pv.yml
cat mongodb-pvc.yml
cat mongodb-deploy.yml
oc apply -f mongodb-pv.yml
oc apply -f mongodb-pvc.yml
oc apply -f mongodb-deploy.yml
oc get po -w
oc get po
oc logs 
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/f7c86bcc-d9ea-4b65-9f7b-66301465a011)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/3c37703a-686c-46bc-a003-31c279f2ec68)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/61bec7db-7f6a-4033-a14a-4d54da959373)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/48bd626b-7098-45d8-b8a2-80cd84b04c71)

## Lab - Running a one time task as an Openshift Job
```
cd ~/openshift-june-2024
git pull
cd Day3/job
cat job.yml
oc apply -f job.yml
oc get jobs
oc logs -f job/hello-job
```

## Lab - Running recurring CronJob at a specified time
```
cd ~/openshift-june-2024
git pull
cd Day3/cronjob
cat cronjob.yml
oc apply -f cronjob.yml
oc get cronjobs
oc get po
oc logs -f cron-job-28657823-p6bgx
```

Expected output
<pre>
jegan@tektutor.org $ oc apply -f cronjob.yml
cronjob.batch/cron-job configured
  
jegan@tektutor.org $ oc get po -w
NAME                      READY   STATUS      RESTARTS   AGE
cron-job-28657815-fnqbl   0/1     Completed   0          6m56s
hello-job-km8pg           0/1     Completed   0          47m
pause-5df6b47b66-zjvpl    1/1     Running     0          49m
cron-job-28657823-p6bgx   0/1     Pending     0          0s
cron-job-28657823-p6bgx   0/1     Pending     0          0s
cron-job-28657823-p6bgx   0/1     Pending     0          0s
cron-job-28657823-p6bgx   0/1     ContainerCreating   0          0s
cron-job-28657823-p6bgx   0/1     ContainerCreating   0          0s
cron-job-28657823-p6bgx   0/1     Completed           0          3s
cron-job-28657823-p6bgx   0/1     Completed           0          4s
cron-job-28657823-p6bgx   0/1     Completed           0          5s
cron-job-28657823-p6bgx   0/1     Completed           0          5s
  
jegan@tektutor.org $ oc logs -f cron-job-28657823-p6bgx
Hello  
</pre>
