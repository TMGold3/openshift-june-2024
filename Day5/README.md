# Day 5

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


