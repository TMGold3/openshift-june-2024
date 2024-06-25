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

