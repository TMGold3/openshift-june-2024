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

```

Expected output
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/e7c85ba4-1b16-450a-afe3-bbc178f91674)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/54ef3a1f-418d-4a0a-a13e-d64113a32ae4)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/cf8bd4af-70bf-4d7d-b4aa-8d4c338d4f56)
![image](https://github.com/tektutor/openshift-june-2024/assets/12674043/fa0f3087-b9db-448a-9e9b-91d1f8b73e12)
