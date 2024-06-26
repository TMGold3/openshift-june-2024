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
