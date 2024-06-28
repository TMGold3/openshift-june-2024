# Day 5

## Post Test Link
<pre>
https://rpsconsulting116.examly.io/contest/publicU2FsdGVkX1+lj2JvRF3+bex/ua1NkTg4RWPsa3kSjTgsunMnwnzHAvmgEYl90cBn7Oz9grc0QNwV933IJwQubQ==
</pre>

## Feedback link
<pre>
https://survey.zohopublic.com/zs/M50Mel
</pre>

## Creating a Docker Registry in your JFrog Artifactory Cloud environment
Login to your JFrog Artifactory Cloud account
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/881eef7e-520d-4aa3-a224-a64ed03a98b0)

Switch to Administration Tab
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/a8682a07-1002-4f7f-b04e-57f86969d6cc)

Click on Repositories on the left side menu
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/7e903451-c1a3-4fab-bc08-0f672d1ed09e)

Click on Create Repository menu on the top right corner
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/69ce1954-dd41-4e03-8d85-bdbbecfd8e8f)

Click on Pre-built Setup
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/2bebe879-28e7-41c3-9cf1-a07508697429)

Select Docker
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/fe46b242-6446-4574-9916-23febadb9b2f)

Type your name and click on create button
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/1fe40c5c-3da4-49f8-9ae4-6ed69569aef3)

Click Continue button when you see a screen as shown in the screenshot below
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/f76cb822-986b-48de-a0c9-0a2e1e90d63b)

Need to click on "Docker Client" Green arrow
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/2864a3fb-8638-4f7b-a1b5-e1ebdab93f25)

Copy the instructions shown below in some text file and save it for your reference, click on Generate
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/8f86fb68-081f-46f2-b40f-655e9280fac4)

Copy the token and save it in the same text file where you saved the previous instructions. Now, we need to execute the docker login instructions shown in the screen on your linux terminal using the token as the password
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/ca9e8921-5410-4801-8999-fc26a93a5900)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/d0073804-3b15-4108-a973-cc154994063f)

Click on Next button, copy the instruction shown below save it in text file and execute the instruction on your linux terminal
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/22b863f3-aba1-4921-91b7-780619c8106e)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/79d603c3-87f8-4a27-a33b-dbb0a226b171)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/182d12f8-a71b-4a20-8cff-662c5e2212de)

Click on Next button,copy the instruction in the text file and execute the pull and tag followed by push command
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/3c6e5a35-418a-417a-a019-d530142dfe81)

Click on Done and you will see the below page. You will notice 
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/69708040-deca-4152-b2fc-ce7882f1e3ef)


## Lab - Build custom image with our application binary and push the image to JFrog Artifactory Docker Registry

Openshift need to login to JFrog Artifactry server in order to push the custom docker images, hence let's create a secret to store the JFrog Login Credentials in Openshift
```
oc create secret docker-registry private-jfrog-image-registry --docker-server=https://<your-jfrog-id>.jfrog.io --docker-username=<your-jfrog-registered-email> --docker-password=<your-jfrog-token>
secret/private-jfrog-image-registry created
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/bbac9735-7515-4b23-badd-144f70dfea05)


We need to create a buildconfig along with the jfrog credentials in the form of pushsecret as shown below
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/e6871f46-2210-4960-a984-56391c93b2a5)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/b435bf82-58d1-4c91-a145-f292dc72f754)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/3c1e03a4-dcbb-44a9-99dc-586415bf79dc)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/5be9cfeb-945b-48c7-9730-eeab23320db5)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/09e08ed1-5a2f-4964-88f8-923eea448658)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/cea84cc8-95d2-4718-ab4f-7bd5d53749aa)


## Lab - Deploying hello microservice using our custom docker image from Private JFrog Docker Registry
```
cd ~/openshift-june-2024
git pull
cd Day5/buildconfig
oc create deployment hello --image=tektutorjegan74.jfrog.io/jegan-docker/hello-spring-microservice:1.0 --replicas=3 -o yaml --dry-run=client

oc create deployment hello --image=tektutorjegan74.jfrog.io/jegan-docker/hello-spring-microservice:1.0 --replicas=3 -o yaml --dry-run=client  > hello-deploy.yml
```

Update the hello-deploy to use your private-jfrog-image-registry secret as JFrog Artifactory will allow only authorized users to download and use the image from it.
```
cd ~/openshift-june-2024
git pull
cd Day5/buildconfig
cat hello-deploy.yml
oc apply -f hello-deploy.yml
oc get deploy,po
oc get po -w
oc get po
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/ade21be2-c7aa-42c1-98d2-0effddd49f8d)

Let's create an internal service for hello microservice
```
oc expose deploy/hello --type=ClusterIP --port=8080 -o yaml --dry-run=client
oc expose deploy/hello --type=ClusterIP --port=8080 -o yaml --dry-run=client > hello-svc.yml
oc apply -f hello-svc.yml
oc get svc
oc describe svc/hello
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/f479b8ff-1c00-41ba-8c56-f7229aa7b2f5)

Let's create a route to access the hello microservice from outside the openshift cluster
```
oc expose svc/hello -o yaml --dry-run=client
oc expose svc/hello -o yaml --dry-run=client > hello-route.yml

cat hello-route.yml

oc apply -f hello-route.yml
oc get route
oc describe route/hello
```
Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/fe2e6376-2bab-4c77-bc35-f8952e6865bf)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/345c8448-5047-4a87-9f71-67553c2acb0d)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/b77d1875-030c-49e6-a727-cec01425f6db)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/4fdb992f-4d9b-4a90-9257-634448579865)

## Lab - Rolling update - updating hello microservice to its v2.0
When the build config file is updated, we need to apply the changes in the openshift cluster
```
cd ~/openshift-june-2024
git pull
cd Day5/buildconfig
cat buildconfig-pushto-artifactory.yml
oc apply -f buildconfig-pushto-artifactory.yml
oc get buildconfigs
oc start-build bc/hello
```
Once the new image v2.0 is pushed successfully to JFrog Artifactory, you can proceed as shown bellow

```
oc set image deploy/hello hello=tektutorjegan74.jfrog.io/jegan-docker/hello-spring-microservice:2.0
oc get deploy/hello -o yaml | grep image
oc get route
curl http://hello-jegan.apps.ocp4.tektutor.org.labs
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/9d78bfd7-990d-4e5b-83ce-df76eaf7fcfa)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/5eaec459-52d0-471b-93b9-1e6cffde06e6)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/2f89be44-5ca0-4e60-a6ef-993fa61d3f03)

## Info - What is Continuous Integration?
<pre>
- Jenkins - is a CI Build Server
- We can create a Jenkins Job - to monitor code commits done in GitHub/BitBucket or any version control
- Whenever Jenkins detects code commit in the version control, it will start the build
- As part of the CI Build, it will first clone the latest source code from GitHub/BitBucket code repository
- Then it will start the application build (maven build, dotnet build)
- As part of the build, you also have to have some automated test cases which runs part of the build
- If any test cases fails, the build will also fail
- If code is compiling and all test cases are executed successfully then the build will succeed.
</pre>

## Info - What is Continuous Deployment?
<pre>
- the dev team certified CI builds, will automatically deploy the application binaries into QA environment for further automated testing
- if all the automated test cases added by QA team succeeds then the build is good to go live in production
- it might require some manual approvals
</pre>

## Info - What is Continuous Delivery?
<pre>
- the QA certified build will automatically be deployed into pre-prod environment for the customer to check and approve to decide to make them live in production  
</pre>

## Info - What is a Jenkins CI/CD Job?
<pre>
- could build application source and run automated test cases
- could build custom docker images
- could deploy application binaries to JFrog, Weblogic or JBoss
- could deploy application into Openshift
</pre>  

## Info - What is Jenkins Pipeline?
<pre>
- Pipelines involves many Jenkins Job that run one after the other in sequence or in parallel
- Pipelines consists of many Stages
- Each Stage will have one Jenkins Job
- When the First Stage Job succeeds it will trigger next downstream jenkins job in the pipeline
- If the second stage Job succeeds it will trigger the next downstream jenkins job in the pipeline
- this goes on until all the jobs complete successfully
- if any one of the stage fails, it won't trigger the next downstream jenkins job and the build will fail
</pre>

## Lab - Deploying Jenkins in Openshift via Helm
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/1c1583e2-6902-4752-8a8d-f3f4dd158475)
Click on Add
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/8f688328-8353-4cd9-85bd-82d995ea6353)
Click on Developer Catalog --> All Service
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/7b0e235b-ebb7-49f2-ac15-b4b880e6d33a)
Search jenkins and select Jenkins Helm Charts
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/ddd13b8f-f2c7-4d0a-8bf2-adc2ac5f9b7f)
Click on Create when you see the below screen
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/0f1848b7-3ec9-4e44-81fe-9da3a89da4da)
Accept the default values and click on Create button
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/b97a03f6-d730-48b8-84f6-be2ee87f7c4d)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/55dd4fb1-d48a-4fb4-a625-5aa879117515)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/2e5ef0e7-bd10-452c-8db2-446aaa51ef8e)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/37365bb3-3a31-42e2-9610-e7c222e1ef34)
Click on "Login with Openshift"
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/3e2e3738-860d-4507-95b7-1a2df5ad7ea6)

## What makes the Serveless architecture work in Openshift or Kubernetes
<pre>
- You need to install OpenShift Serverless Operator
- The Serverless Operator installs knative serverless framework
</pre>

## Knative and Red Hat Servless
<pre>
- Red Hat Serverless is based on Knative opensource project
- Knative provides a serverless application layer on top of OpenShift/Kubernetes
- Knative consists of 3 building blocks
  - Build
  - Eventing
  - Serving
</pre>  

## What does Serverless mean ?
<pre>
- serverless doesn not mean the absence of servers
- is an architecture model for running applications in an environment that is abstracted away from developers
- developers can focus more on developing their application that where their code runs
- an ideal serverless workload executes a single task
- a function that retrieves data from a database can be an execellent serverless workload
- serverless model is the idea of the cold start
- when using serverless, there is a period between the request and creating the pod environment.  This period is called cold start.
- Examples
  - openshift serverless workloads follow this workflow
    - a request comes in
    - a pod is spun up to service the request
    - the serves the request
    - the pod is destroy when there is no user traffic to handle
    - your service will be scaled down all the way upto 0 pod when there is 0 zero
  - Another example of a serverless workload can be an image processing function
    - an event could be a photo upload. The uploaded photo triggers an event to run an application to process the image
    - For example, the application may overtext text, create a banner, or make thumbnail
    - Once the image is stored permanently, the application has served its purpose and is no longer needed
</pre>

## Serverless Features
<pre>
- Stateless Function
  - a function to query a database and return the data
  - a function to query weather report and return the data
- Event Driven
  - a serverless model relies on a trigger to execute the code
  - could be a request to an API or an event on a queue
- Auto Scales to zero
  - Being able to scale to zero means your code only runs when it needs to respond to an event
  - once the request is serverd, resources are release
</pre>

## Lab - Installing Red Hat OpenShift Serverless Operator as Openshift Administrator
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/24dd1417-6191-4655-9209-45354bfad194)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/f66cd818-a0a0-46e6-869a-18f33f633b90)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/84313b25-54c3-4370-b498-2b39e2b7db3e)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/39bfab09-eda5-46cc-8dc6-17ef9280124f)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/c5921665-c074-44db-ae57-df4aeaa42ab8)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/bd134454-67bd-400b-9675-9879dd435bdd)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/17058bbb-4e55-4257-91eb-f6c7aa102188)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/481c8d7b-dbc7-4008-ad7b-6e4f3ba3b4b2)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/084400aa-99a0-4a47-b548-e39cb85c31ff)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/795c14e9-11ab-4ec2-af20-3a9a7d4d7df9)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/ee545abe-649e-4a85-8c93-074a41513f2b)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/d481df9c-a6ed-458c-854e-89183c610e05)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/247411fe-a1c6-4530-8760-dc2e8549dfb1)


## Lab - Deploying your first knative service
```
kn service create hello \
--image ghcr.io/knative/helloworld-go:latest \
--port 8080
--env TARGET=World
```

Accessing the knative application from command line
```
curl -k https://hello-jegan-serverless.apps.ocp4.tektutor.org.labs
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/6e787cda-1803-455f-940b-d1f5c6a3e90b)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/f94e2526-a0f6-4fea-adb7-a5675768c03a)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/c43ec123-409b-4cb4-8f2d-565bb9c55bd8)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/01c3a7e9-104e-4fdd-9d9a-fa1b59b196d9)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/bff87680-cdd8-47b5-a841-bdef44c0ed1d)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/227fa63d-a7d5-435b-9c89-cf22ace3d3e4)

#### Update the service
```
kn service update hello --env TARGET=Knative!
kn revisions list
```

Accessing the knative application from command line
```
curl -k https://hello-jegan-serverless.apps.ocp4.tektutor.org.labs
```


Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/754f93dc-f591-4e16-9b61-2cdbf7c32e6a)


Splitting the traffic between two revisions
```
kn service update hello --traffic hello-00001=50 --traffic @latest=50
kn revisions list
```
Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/cbd07e1f-e838-41f6-a80a-edc5f8273506)


Delete the knative service
```
kn service list
kn service delete hello
kn service list
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/48ecedc4-ef88-4124-90d2-c44ef9695171)

## Lab - Knative eventing

Let's deploy a sink service
```
oc project jegan-serverless
kn service create eventinghello --concurrency-target=1 --image=quay.io/rhdevelopers/eventinghello:0.0.2
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/af80d323-d397-4671-ad37-a7345d983561)

Let's create an event source application
```
kn source ping create eventinghello-ping-source --schedule="*/2 * * * *" --data '{"message": "Thanks for your message"}' --sink ksvc:eventinghello
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/b194ce5b-6a5b-4f47-9427-d64cabd1ca12)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/bea18764-b175-434a-920f-91f8d0cd8964)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/803b22ec-24d7-40ab-93d9-c55d999975de)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/03310fc3-5a27-4b35-a27a-f76a5ae6dce1)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/2c26ab49-a6d1-4944-9131-5e0b5e5a5d4a)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/8ac58fa6-49a8-4158-8a08-0ef689f35d45)

## Lab - Developing a simple knative function in nodejs and deploying into Openshift cluster

This will generate a basic nodejs application in your current directory
```
kn func create -l node
```

If you wish to build your application
```
kn func build
```

If you wish to run the application locally and test it
```
kn func run
```

Deploy the nodejs application into openshift after building it
```
kn func deploy -n jegan-serverless
```

Test the knative function
```
curl -k https://functions-jegan.apps.ocp4.tektutor.org.labs
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/590d13b7-beaf-4aff-8c07-71ad9bac8da8)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/384f420c-620b-4bec-9865-954f0252b5bf)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/4ab8b10b-7c11-4f11-b6b4-d43d5bfdf176)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/f84608c1-9e4c-471c-8348-e2d005fb6a8b)
