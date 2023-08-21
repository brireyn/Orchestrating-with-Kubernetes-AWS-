# Orchestrating-with-Kubernetes-AWS-


<br> <br>
<h2>Description</h2>
This lab demonstrates provisioning a complete Kubernetes cluster using Kubernetes Engine.  Deploy and manage Docker containers using kubectl.  Breaking an application into microservices using Kubernetes' Deployments and Services.


<h1>Orchestrating the cloud with Kubernetes </h1>

<h2>Environments Used </h2>
- <b>Kubernetes Engine</b>
- <b>Windows 10</b>
- <b>Google Cloud Console</b>
- <b>Qwiklabs</b>

<h2>Program walk-through:</h2>

<p align="center">

<h3>Task 1:</h3>
1. Set the zone and io for the container clusters.
![Step1](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/2bb61cb4-3e5e-4612-b56a-5a9be53e3abe)
![io_configured](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/f48b0546-89d0-4b61-a179-48e70d34303a)

2. Get the Sample code with gsutil cp -r gs://spls/gsp021/* .
3. ![Get_Sample_code](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/2d263e10-3a9b-4d84-9dea-4d48637ab4ee)

4. Change into the directory used for the lab and list the files working on:  cd orchestrate-with-kubernetes/kubernetes
5. ls   ![layout_code](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/e2706dc4-897d-4711-b80d-d8503e13ecf1)

<h3>Task 2:</h3> 
1.  Use (kubectl create deployment nginx --image=nginx:1.10.0) to launch a single instance of the nginx container.

2.  Use the kubectl get pods command to view the running nginx container.
![step3_get_pods](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/5c92e47b-5f49-4d3a-92ec-520bcf06ad87)

3.  Once the nginx container has a Running status you can expose it outside of Kubernetes using the kubectl expose command: kubectl expose deployment nginx --port 80 --type LoadBalancer

![step3_expose_deployment](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/fdb8cb64-0bab-4adb-b1c4-dda06c61021d)

4. Kubernetes created an external Load Balancer in AWS with a public IP address attached to it. When a client hits that public IP address will be routed to the pods behind the service. In this case that would be the nginx pod.
- List our services now using the kubectl get services command:

kubectl get services
![step3_get_services](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/7366fe49-55d5-4650-a83b-214aab306491)


5.  Add the External IP to this command to hit the Nginx container remotely:

curl http://<External IP>:80  

<h3>Task 4: Creating Pods</h3>

Displays monolith pod configuration file.
1. Go to directory: cd ~/orchestrate-with-kubernetes/kubernetes
2. Run the following: cat pods/monolith.yaml
![task4_creating_pods](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/165a0e81-dfbf-4abc-8b04-cd02830d1d88)
*This displays it is running 1 monolith ( one container), passing a few arguments when starting up and its opening port 80 for http traffic.

3. Create the monolith pod: kubectl create -f pods/monolith.yaml
4. Examine your pods. Use the kubectl get pods command to list all pods running in the default namespace: kubectl get pods
5. Once the pod is running, use kubectl describe command to get more information about the monolith pod: kubectl describe pods monolith
   ![create_monolith_pod_ex](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/fbe97522-6d98-4194-a665-52cc1f9a455d)
![pod_info](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/37af4d7d-6197-48c6-bb9a-e0a43c55c171)

<h3>Task 5: Interacting with Pods</h3>
1. In a 2nd Cloud Shell Terminal run: kubectl port-forward monolith 10080:80   (for port forwarding)

![task5_port_fwd](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/89ca9557-590a-40ae-ad4c-ff32a447a876)

2. In the first terminal start communicating to the pod using curl: curl http://127.0.0.1:10080
   ![hello_message](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/45c0500b-c4f3-4ee3-ab78-d18191361967)
( will recieve a "Hello" back from the container.) 

3. Hit a secure endpoint with curl http://127.0.0.1:10080/secure   (endpoints gets denied)
   
![endpoint_denied](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/c8e0573c-7603-445e-ac37-8f568d0b1698)
![hit_endpoint](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/577178a8-b30d-44b9-92d1-911a5303b4e9)

4. Log in to get an auth token back from  the monolith. curl -u user http://127.0.0.1:10080/login
   ![auth_token_login](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/5d29cb98-5424-49be-babe-be0874bc0942)

5. 









<br />


<br />
 <br/>

</p>
