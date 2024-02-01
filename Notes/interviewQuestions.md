# Interview Questions and Answer

##### Question 1: 
What is the difference between Docker and Kubernetes?
##### Answer: 
- Docker is a container platform whereas Kubernetes is a container orchestration enviornment that offers capabilities like Auto Healing, Auto Scaling, Clustering and Enterprise level support like Load Balancing, etc.

##### Question 2: 
What are the main components of the Kubernetes architecture?
##### Answer:
- On a broad level, you can divide the kubernetes components into two parts. 
- The first part is the Control Plane (API Server, Scheduler, Controller Manager, Cloud-controller Manager, etcd).
- The second part is the Data Plane(kubelet, Kube-proxy, Container Runtime).

##### Question 3: 
What are the main difference between the Docker Swarm and the Kubernetes?
##### Answer:
- Kubernetes is better suited for large organisations as it offers more scalabilty, networking capabilities like policies and huge third-party ecosystem support.

##### Question 4: 
What is the difference betweeen Docker container and a Kubernetes pod?
##### Answer:
- A pod in kubernetes is a runtime specification of a container in docker. A pod provides more declarative way of defining using YAML and you can run more than one container in a pod.

##### Question 5:
What is a namespace in Kubernetes?
##### Answer:
- In kubernetes namespace is a logical isolation of resources , network policies, rbac and everything. For example, there are two projects using same K8s cluster. One project can use ns1 and other project can use ns2 without any overlap and authetication problem.

##### Question 6:
What is the role of kube proxy?
##### Answer:
- Kube-proxy works by maintaining a set of network rules on each node in the cluster, which are updated dynamically as services are added or removed. When a client sends a request to a service, the request is intercepted by kube-proxy on the node where it was recieved. Kube-proxy on the node where it was recieved. Kube-proxy then looks up the destination endpoint for the service and routes the request accordingly.
- Kube-proxy is an essential component of a Kubernetes cluster as it ensures that services can communicate with each other.

##### Question 7:
What are the different type of services in Kubernetes?
##### Answer:
- There are three type of services that a user can create is:
1. Cluster IP Mode
2. Node Port Mode
3. Load Balancer Mode

##### Question 8:
What is the difference between NodePort and LoadBalancer type services?
##### Answer:
- When a service is created a NodePort type, The kube-proxy updates the ipTables with Node IP address and port that is chosen in the service configuration to access the pods.
- Where as if you create a Service as type LoadBalancer, the cloud control manager creates a external load balancer IP using the undelying cloud provider logic in C-CM. Users can access services using the external IP.

##### Question 9:
What is the role of Kubelet?
##### Answer:
- Kubelet manages the containers that are scheduled to run on that node. It ensures that the containers are running and healthy, and that the resources they need are available.
- Kubelet communicates with the kubernetes API server to get information about the containers that should be running on the node, and then starts and stops the containers as needed to maintain the desired state. It also monitors the containers to ensure that they are running correctly, and restarts them if necessary.

##### Question 10:
What are day to day activities on Kubernetes?
##### Answer:
- As part of the DevOps engineer role we manage kubernetes cluster and we also ensure that you the application are deloyed onto the kubernetes cluster and there are no issues with the application so we have setup the monitoring on the kubernetes cluster we ensure that whenever there are the bugs on the kubernetes cluster then devops engineer comes into the picture and we also do a lot of maintenance activities.