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
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/5f153733-6305-412e-a2ef-3c7497287139)
