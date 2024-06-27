# Day 4

## Info - Helm Overview
<pre>
- Just we have package manage in Linux distributions
  - apt-get in ubuntu
  - yum in Red Hat based Linux distributions
- helm  is a package manager for Kubernetes and Openshift 
- using helm we can install, uninstall, ugrade applications into Kubernetes and Openshift
- helm works just like oc or kubectl outside the cluster as a client tool
- using helm we can package our yaml manifest scripts in a specific format called Helm Chart
- Helm chart is a compressed file which can distribute to your customers
- Using the Helm chart, your customer can install your application into Kubernetes and Openshift
- There is a community maintained Helm chart portal is there, you can download charts other people developed and published
</pre>

## Lab - Creating a custom helm chart to package the wordpress & mariadb multipod application
```
cd ~/openshift-june-2024
git pull
cd Day4/helm
helm create wordpress
tree wordpress
cd wordpress/templates
rm -rf *
cd ..
echo "" > values.yaml
cp ../scripts/*.yml templates
cp ../values.yaml .
cd ..
helm package wordpress
ls -l
helm install wordpress wordpress-0.1.0.tgz
helm list
```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/e7c85ba4-1b16-450a-afe3-bbc178f91674)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/54ef3a1f-418d-4a0a-a13e-d64113a32ae4)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/cf8bd4af-70bf-4d7d-b4aa-8d4c338d4f56)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/fa0f3087-b9db-448a-9e9b-91d1f8b73e12)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/4f9c15a4-9e70-43b9-abda-6ae29f7950d7)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/fbe3f866-d076-4428-9ec2-f37c524d3365)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/e2a2c70b-66d4-46aa-b714-8a68e1b143af)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/e2d6914d-e16f-4e81-8c69-b54212c3dda3)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/b08f3e3e-6dfe-4558-be2f-8b575cffac6e)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/8d57cb7b-8f15-441b-bfc0-71ffae547dff)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/7deb2f83-b095-4a8f-9bd8-047166d9791f)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/a0e873b9-a2fe-422a-9f26-9b1246bb64eb)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/77cd86dc-b3d6-4e49-a501-f4a745ab17fa)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/f87844f0-4290-4440-989a-c5c7e109e492)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/c93a4e00-a59a-4485-9a64-114466f57b47)
