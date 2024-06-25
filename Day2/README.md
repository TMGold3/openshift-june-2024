# Day 2

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
