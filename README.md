# Kubernetes-basics

**What is Kubernetes:**\
Years ago, most software applications were big monoliths, running either as a single process or as a small number of processes spread across a handful of servers.
Today, these big monolithic legacy applications are slowly being broken down into smaller, independently running components called microservices.
Because microservices are decoupled from each other, they can be developed, deployed, updated, and scaled individually. This enables you to change components quickly and as often as necessary to keep up with today’s rapidly changing business requirements.

Kubernetes in an open source container management tool hosted by Cloud Native Computing Foundation (CNCF). This is also known as the enhanced version of Borg which was developed at Google to manage both long running processes and batch jobs, which was earlier handled by separate systems. Kubernetes comes with a capability of automating deployment, scaling of application, and operations of application containers across clusters. It is capable of creating container centric infrastructure

**FEATURES:**\
•	Load balancing \
•	Self-healing(automatic restarts)\  
•	Scheduling \
•	Scaling \
•	Rolling updates\
**Splitting apps into microservice** \
Each microservice runs as an independent process and communicates with other microservices through simple, well-defined interfaces (APIs). Refer to the image below:
![image](https://user-images.githubusercontent.com/82542326/127751701-370c4cc2-3f99-45eb-9d1c-ac37644f1f9e.png)

 - Image taken from other source.
Scaling Microservices
Scaling microservices, unlike monolithic systems, where you need to scale the system as a whole, is done on a per-service basis, which means you have the option of scaling only those services that require more resources, while leaving others at their original scale. Refer to the image below:
 ![image](https://user-images.githubusercontent.com/82542326/127751714-e0d25bbb-a078-4a0e-9f74-fd921ec6aa89.png)

- Image taken from other source.

**Deploying Microservices**
As always, microservices also have drawbacks. When your system consists of only a small number of deployable components, managing those components is easy. It’s trivial to decide where to deploy each component, because there aren’t that many choices.
When the number of those components increases, deployment-related decisions become increasingly difficult because not only does the number of deployment combinations increase, but the number of inter-dependencies between the components increases by an even greater factor.
Microservices also bring other problems, such as making it hard to debug and trace execution calls, because they span multiple processes and machines. Luckily, these problems are now being addressed with distributed tracing systems such as Zipkin.\
 ![image](https://user-images.githubusercontent.com/82542326/127751717-cb75e092-7a2a-4872-b584-0d933b19bf3b.png)

Multiple applications running on the same host may have conflicting dependencies.

**Kubernetes architecture:**\
![image](https://user-images.githubusercontent.com/82542326/127751721-71967673-622e-43d7-8df5-52b6915b5f3b.png)

 
**Kubernetes master:**\
![image](https://user-images.githubusercontent.com/82542326/127751726-5249c38b-70ce-4300-9622-689790c3c75d.png)

 
**MASTER COMPONENTS:**
- Etcd - it is a key-value data store used to store configuration info which can be used by each of node in the cluster. It is high availability key value store that can be distributed among nodes. It is accessed only by kubernetes api server, because of sensitive info.
- Api server - kubernetes is an API which provides all operation using an API. Api server implementes interface, which means uses different tools and libraries which can communicate with it. Kubeconfig is a package in the server side used for communication. It exposes api. It is the only master component that is user accessable. 
- Controller manager - it is a daemon that embeds the corecontrol loops. Basically a controller watches over state of clusters with api server and when its get notified, it makes necessary changes to cluster to bring to desired state. It has node controller, service controller, replication controller. Each of these controllers works separately to maintain desired state. 
- Scheduler - it is a service in master which is responsible for sharing workload among nodes. The scheduler watches newly created pods and assigned them to nodes.

**NODE:** 
- Nodes are also called as minions, where all our work is happening.  
**Node Components**
- Kubelet - it is the most important component in kubernetes. Basically it’s an agent running on nodes which watches api server for the pods that are bound to its node. It makes sure that pods are running. It reports to api server about status of pods. Exposes endpoint on: 10255
- Container Engine - it does container management. Like pulling images, start/stop containers. It is pluggable, you can use docker, rocket etc. 
- Kube proxy - it assigns ip address to pods. Pod will have one ip address. All containers in pod wil share one ip. It load balances across all pods. It builds bunch of iptables rules and reference the portal ip and insert those rules to the node which it lives on. 
- We will write a manifest file which has our desired state and give it to api server. The api will take care to get the desired state.
- The desired state is maintained by api. If the nodes are not in desired state, api will get the state by scaling.

-	Kubeadm - command to bootstrap the cluster.
-	Kubelet - agent which runs on all nodes, reports to master. 
-	Kubectl - cli to talk to your cluster

**PODS:**
- A pod is a group of containers that are deployed together on a single host. 
- Pods are resides under nodes. More than one pod can share the same node. 
- We hav eto write manifest files to create and run pods. These manifest file are executed by api server in nodes. 
- Kubectl get pods - to get list of pods. 
- Kubectl create –f yml-file - to create a pod (-f = file) 
- Kubectl describe pods - get detailed info. 
- Kubectl get pods/podname - to see specific pod. 
- Kubectl delete pods/pod-name - to delete a pod. 
- Kubectl apply –f yml-file - to update the pods (replication controller). 
- Updating the pods means, when you update the manifest file to scale up (or) down the pods you have to use apply command. Because you have already created those pods.

**CONTROLLERS:**
- We specify replicas in controllers. And we specify our container in template section with a label. 
- Kubectl get rc - to get replication controllers 
- Kubectl get rc rc-name - to get specific rc. 
- Kubectl describe rc rc-name - shows details of pod status, replicas etc. 

**SERVICES:**
- Service discovery 
- Dns based • Environment variables 
- Service type 
- Cluster ip - stable inter cluster ip 
- Node port - exposes the app outside of cluster by adding a clusterwide open port on top of cluster. (the container port is exposed to outside world via node port). 
- Load balancers - integrates nodeport with cloud based lb’s. 
- Once you created the service, it will load balance across your pods and you can access pods outside of your cluster. 
- You can also use cloud based load balancers with kubernetes. 
- Kubectl get svc - to get services 
- Kubectl describe svc svc-name - to get detailed info of a service. 
- Kubectl delete svc svc-name - to delete a service. 

**Deployments:**

- Deployments use replica sets, which have an unique feature calles revision. 
- It stores version of your manifest file and you can go back to any version you want to rolling updates.
- The only thing that u need to remember is the version number. 
- here are no deployments in apiversion 1. You have to change the api version which is suitable for deployments. 
- You have to specify replica sets in deployments instead of replication controllers,where you can specify minreadyseconds, which means you have to specify a time and if the pod is not up in that time, it will considerthe pod as unhealthy. 
- You have to specify strategy, where you can specify rolling updates. 
- Kubectl get deploy - displays all deployments. 
- Kubectl describe deploy deploy-name - displays details of deployment. 
- Kubectl get rs - displays replica sets 
- Kubectl describe rs - displays details of replica sets. 
- Kubectl apply –f deploy-file --record - records revisions of deployments. 
- Kubectl rollout history deploy deploy-name - displays deployment revisions. 
- Kubectl rollout status deploy deploy-name - to see the status of last rolledout deployment. 
- Kubectl rollout undo deploy deploy-name --to-revision=3 - to roll back to specific revision number.
