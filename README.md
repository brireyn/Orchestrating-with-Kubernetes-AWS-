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
6. List of files that we are working with ![listed_files_working_with](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/884d2b02-9d1e-4fdc-8ed3-5a0e5163c208)


<h3>Task 2:</h3> 
1.  Use (kubectl create deployment nginx --image=nginx:1.10.0) to launch a single instance of the nginx container.

2.  Use the kubectl get pods command to view the running nginx container.
![step3_get_pods](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/5c92e47b-5f49-4d3a-92ec-520bcf06ad87)

3.  Once the nginx container has a Running status you can expose it outside of Kubernetes using the kubectl expose command: kubectl expose deployment nginx --port 80 --type LoadBalancer

![step3_expose_deployment](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/fdb8cb64-0bab-4adb-b1c4-dda06c61021d)

4. Kubernetes created an external Load Balancer in AWS with a public IP address attached to it. When a client hits that public IP address will be routed to the pods behind the service. In this case that would be the nginx pod.
<h3>Task 3</h3>
   
List the services now using the kubectl get services command: kubectl get services
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

![open_2nd_terminal](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/2dd38ed9-cb40-414c-9d66-c86bdbdb4643)

![task5_port_fwd](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/89ca9557-590a-40ae-ad4c-ff32a447a876)

2. In the first terminal start communicating to the pod using curl: curl http://127.0.0.1:10080
   ![hello_message](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/45c0500b-c4f3-4ee3-ab78-d18191361967)
( will recieve a "Hello" back from the container.) 

3. Hit a secure endpoint with curl http://127.0.0.1:10080/secure   (endpoints gets denied)
   
![endpoint_denied](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/c8e0573c-7603-445e-ac37-8f568d0b1698)
![hit_endpoint](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/577178a8-b30d-44b9-92d1-911a5303b4e9)

4. Log in to get an auth token back from  the monolith. <curl -u user http://127.0.0.1:10080/login>
   ![auth_token_login](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/5d29cb98-5424-49be-babe-be0874bc0942)

5. Logging in caused a JWT token to print out, create an environment variable for the token. <TOKEN=$(curl http://127.0.0.1:10080/login -u user|jq -r '.token')>

6. Use this command to copy and then use the token to hit the secure endpoint with curl:

curl -H "Authorization: Bearer $TOKEN" http://127.0.0.1:10080/secure
![auth_token_login](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/eb222a71-2113-4e00-9b6f-ce4e99b3f49c)
-this gives a response back that everything is working.

7. Use the kubectl logs command to view the logs for the monolith Pod.

kubectl logs monolith

8. Open a 3rd terminal and use the -f flag to get a stream of the logs happening in real-time: kubectl logs -f monolith

![logs updating](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/de76792f-2acd-4c39-8fec-b2c824a6ebf8)
![step11_mono_logs](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/0a54ead5-5221-4fe8-a877-22789826b8e6)

9. Now if you use curl in the 1st terminal to interact with the monolith, you can see the logs updating (in the 3rd terminal):  curl http://127.0.0.1:10080
10. Use the kubectl exec command to run an interactive shell inside the Monolith Pod. This can come in handy when you want to troubleshoot from within a container:
ping -c 3 google.com
![ping_google](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/96812ce0-2510-49c6-beca-631e3eef2403)
 Then exit.

<h3>Task 6:</h3> Services: Create a service and use label selectors to expose a limited set of Pods externally.

<h3>Task 7:</h3> Creating a service

Before you can create our services, first create a secure pod that can handle https traffic.

1. If you've changed directories, make sure you return to the ~/orchestrate-with-kubernetes/kubernetes directory:

cd ~/orchestrate-with-kubernetes/kubernetes

2. Explore the monolith service configuration file:  cat pods/secure-monolith.yaml
3. Create the secure-monolith pods and their configuration data:

kubectl create secret generic tls-certs --from-file tls/
kubectl create configmap nginx-proxy-conf --from-file nginx/proxy.conf
kubectl create -f pods/secure-monolith.yaml

4. The monolith service configuration file: cat services/monolith.yaml
(Output) ![monolith](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/080b93b9-c6ca-4e36-946f-d11cc4aa01d3)

* There's a selector which is used to automatically find and expose any pods with the labels `app: monolith` and `secure: enabled`.

* Now you have to expose the nodeport here because this is how you'll forward external traffic from port 31000 to nginx (on port 443).



5. Use the kubectl create command to create the monolith service from the monolith service configuration file:  kubectl create -f services/monolith.yaml
![monolith_created](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/59296d91-8a19-43d0-8fdd-c40b6c9bf1e8)

6. Use the gcloud compute firewall-rules command to allow traffic to the monolith service on the exposed nodeport:
   ![task_7_firewall](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/462bfff2-55f9-4384-9d23-9fe13a6a0678)

This allows traffic to the monolith service on the exposed nodeport from outside the cluster using port forwarding:

1. First, get an external IP address for one of the nodes : gcloud compute instances list
2. Now try hitting the secure-monolith service using curl: curl -k https://<EXTERNAL_IP>:31000
   
![not_working_task7](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/a9098769-6c32-4e71-8c6d-34e921697bcf)

Will get an error since there is an issue with the labels. Need to add the labels to the pods.

<h3>Task 8: Adding Labels to pods</h3> 
1. Currently the monolith service does not have endpoints. One way to troubleshoot an issue like this is to use the kubectl get pods command with a label query. kubectl get pods -l "app=monolith"   

![task8_get_pods](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/24130f6b-b39c-429a-8ff5-e97946de8099)

2.  kubectl get pods -l "app=monolith,secure=enabled" 
![task8_2](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/77ee32f5-4ec9-4b98-b5bb-0ea04b4cd009)

3. Use the kubectl label command to add the missing secure=enabled label to the secure-monolith Pod. Afterwards, you can check and see that your labels have been updated.
   kubectl label pods secure-monolith 'secure=enabled'
   kubectl get pods secure-monolith --show-labels
   ![task8_3](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/dfdca5dd-8cdc-4a55-b24e-08f8ce71063e)


4. Now that the pods are correctly labeled, view the list of endpoints on the monolith service: kubectl describe services monolith | grep Endpoints
![task8_4](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/7515ce94-d62b-4eac-b713-fe17609dec1a)

5. Test this out by hitting one of our nodes again and it shows contact. 
    ![task8_5](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/f483b895-5407-446e-ab5b-356f3e20beb6)

<h3>Task 9: Deploying Applications with Kubernetes</h3>
![task9](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/481a7a09-0dc2-4ee6-a8f8-8342b75a89a2)

<h3>Task 10:Creating deployments</h3>
Breaks the monolith app into three separate pieces:
• auth - Generates JWT tokens for authenticated users.
• hello - Greet authenticated users.
• frontend - Routes traffic to the auth and hello services.
*Creating deployments, one for each service.  Afterwards, will define internal services for the auth and hello deployments and an external service for the frontend deployment. Once finished will be able to interact with the microservices just like with Monolith only now each piece will be able to be scaled and deployed, independently.

1. Get started by examining the auth deployment configuration file. cat deployments/auth.yaml
The deployment is creating 1 replica, and you're using version 2.0.0 of the auth container.
![task10_1](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/630ac3bc-4258-43f7-8865-2c90c1c3379c)

When you run the kubectl create command to create the auth deployment it will make one pod that conforms to the data in the Deployment manifest. This means you can scale the number of Pods by changing the number specified in the Replicas field.

2. Create deployment object: kubectl create -f deployments/auth.yaml
   ![task10_2](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/e4691fd4-2e4c-4882-8ca1-f1685f662cad)
3. Create a service for the auth deployment. Use kubectl create -f services/auth.yaml
4. Create and expose the hello deployment:
   kubectl create -f deployments/hello.yaml
   kubectl create -f services/hello.yaml

5. And one more time to create and expose the frontend Deployment.
![task10_5](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/7fcd4c4f-4991-4ec8-b113-d36ef047d97f)

6.Interact with the frontend by grabbing its External IP and then curling to it:

![task10_6](https://github.com/brireyn/Orchestrating-with-Kubernetes-AWS-/assets/96150916/653dfd1a-9b45-4d11-b54f-e9bde2cdb993)

curl -k https://<EXTERNAL-IP>  

Finally, this command gives a "hello" response back. 

This developed a multi-service application using Kubernetes. 













<br />


<br />
 <br/>

</p>
