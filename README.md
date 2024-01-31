# Kubernetes-Notes-And-Projects

### Introduction to Kubernetes: An Enterprise-Grade Container Orchestration Platform

**Why Kubernetes?**

In the dynamic world of containerized applications, Kubernetes emerges as a powerful orchestration platform, addressing critical challenges associated with container management. Containers, known for their ephemeral nature, necessitate a robust solution to manage their lifecycle, resolve issues, and optimize performance.

**Challenges with Containers:**

*Problem 1 - Isolation and Impact:*
Containers on a single host can impact each other, creating isolation challenges. Kubernetes, with its cluster architecture, provides an effective solution by intelligently distributing containers across nodes, mitigating potential conflicts.

*Problem 2 - Auto Healing:*
Auto healing is a crucial aspect where containers autonomously recover from failures without manual intervention. Kubernetes incorporates a sophisticated replica set mechanism that enables seamless auto-scaling and healing, ensuring continuous application availability.

*Problem 3 - Auto-Scaling:*
Scaling applications in response to varying workloads is simplified with Kubernetes. The platform's replica sets and scaling capabilities allow automatic adjustments to match demand, optimizing resource utilization and performance.

*Problem 4 - Enterprise-Level Support:*
Docker, while simple and minimalistic, lacks inherent enterprise-level features. Kubernetes steps in as a comprehensive solution, offering built-in support for load balancing, firewall management, auto-scaling, API gateway integration, and more.

**Kubernetes Solutions:**

*Problem 1 Solution - Cluster Architecture:*
Kubernetes operates on a cluster architecture, distributing containers across nodes. This ensures optimal resource utilization, addressing the challenge of container isolation and impact on a single host.

*Problem 2 Solution - Replica Sets for Auto-Scaling:*
Kubernetes leverages replica sets to facilitate auto-scaling, dynamically adjusting the number of container instances based on workload demands. This ensures applications seamlessly adapt to varying resource requirements.

*Problem 3 Solution - Auto Healing:*
With its control mechanisms, Kubernetes detects and rectifies container failures, facilitating auto healing. This results in increased application reliability and minimizes downtime due to unexpected issues.

*Problem 4 Solution - Enterprise-Grade Container Orchestration:*
As a product of Google, Kubernetes is designed to meet enterprise-level requirements. It provides a comprehensive solution by handling load balancing, firewall management, auto-scaling, API gateway integration, and other critical aspects for a robust container orchestration platform.

# Kubernetes Architecture Overview

## Why "K8s"?

The term "K8s" is a shorthand representation of "Kubernetes," with the number 8 signifying the eight letters between "K" and "s."

## Kubernetes Architecture

### Control Plane

The Control Plane, often referred to as the Master Node, is the brain behind orchestrating the Kubernetes cluster. Its components include:

#### 1. API Server

The **API Server** exposes the Kubernetes API, providing external entities access to the cluster.

#### 2. etcd

**etcd** is a distributed key-value store, serving as a reliable backup for the configuration data of the entire cluster.

#### 3. Scheduler

The **Scheduler** makes decisions on Pod deployment based on available resources, ensuring optimal resource utilization.

#### 4. Controller Manager

The **Controller Manager** monitors the cluster's state, ensuring controllers like Replica Sets are functioning correctly and taking corrective actions when needed.

#### 5. Cloud Controller Manager

In cloud environments (e.g., AWS or EKS), the **Cloud Controller Manager** interfaces with cloud-specific APIs, facilitating integration between the cluster and cloud resources.

### Data Plane (Worker Node)

The Data Plane, residing on Worker Nodes, is responsible for running applications. Key components include:

#### 1. Kubelet

**Kubelet** runs on each Worker Node, managing and maintaining Pods, and communicates with the Control Plane to report Pod status.

#### 2. Kube Proxy

**Kube Proxy** handles networking within the cluster, allocating IP addresses to Pods and providing load balancing capabilities critical for communication and traffic management during auto-scaling.

#### 3. Container Runtime

The **Container Runtime** (e.g., Docker Shim, CRI-O) executes and manages containers within Pods. Kubernetes supports various runtimes, offering flexibility based on the environment.

## Key Functions of Components

- **API Server:** Exposes the Kubernetes API for external interactions.
- **etcd:** Stores cluster configuration data as key-value pairs.
- **Scheduler:** Determines the optimal node for deploying Pods based on available resources.
- **Controller Manager:** Ensures controllers are running and manages cluster state.
- **Cloud Controller Manager:** Integrates Kubernetes with cloud-specific APIs.
- **Kubelet:** Manages and maintains the state of Pods on Worker Nodes.
- **Kube Proxy:** Handles networking, IP allocation, and load balancing within the cluster.
- **Container Runtime:** Executes and manages containers within Pods.

Understanding this architecture provides insights into how Kubernetes efficiently manages containerized applications, offering scalability, resilience, and ease of deployment.

# Managing Kubernetes Clusters with KOPS

## Introduction

In the world of Kubernetes, managing the lifecycle of clusters efficiently is crucial, especially when dealing with production systems. This documentation explores the use of KOPS (Kubernetes Operations), a powerful tool in the DevOps arsenal, for creating, upgrading, and deleting clusters. 

## Kubernetes Distributions

Kubernetes, being an open-source container orchestration platform, has various distributions developed on top of it. Some popular distributions include:

- **Kubernetes (Direct):**
  - Suitable for staging, pre-production, and testing environments.
  
- **EKS (Amazon Elastic Kubernetes Service):**
  - A managed Kubernetes service by Amazon, providing support and additional tools.
  
- **Openshift, Rancher, VMware Tanzu:**
  - These are distributions that enhance the user experience on top of Kubernetes, adding specific features and user-friendly interfaces.

### Choosing Between Kubernetes and EKS

The primary difference lies in support. While installing Kubernetes directly on EC2 instances requires self-management and offers no official support, EKS provides managed support from Amazon, along with additional tools and scripts.

## Managing Multiple Clusters

### Tools Used

- **KOPS (Kubernetes Operations):**
  - KOPS is a versatile tool that aids in managing the lifecycle of Kubernetes clusters. It simplifies cluster creation, upgrades, and deletion.

- **Other Tools (e.g., kubeadm):**
  - Various tools are available for managing Kubernetes clusters, but KOPS stands out for its comprehensive capabilities.

## Kubernetes Installation Using KOPS on EC2

### Create an EC2 instance or use your personal laptop.

Dependencies required 

1. Python3
2. AWS CLI
3. kubectl

###  Install dependencies

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```

```
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
```

```
sudo apt-get update
sudo apt-get install -y python3-pip apt-transport-https kubectl
```

```
pip3 install awscli --upgrade
```

```
export PATH="$PATH:/home/ubuntu/.local/bin/"
```

### Install KOPS (our hero for today)

```
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64

chmod +x kops-linux-amd64

sudo mv kops-linux-amd64 /usr/local/bin/kops
```

### Provide the below permissions to your IAM user. If you are using the admin user, the below permissions are available by default

1. AmazonEC2FullAccess
2. AmazonS3FullAccess
3. IAMFullAccess
4. AmazonVPCFullAccess

### Set up AWS CLI configuration on your EC2 Instance or Laptop.

Run `aws configure`

## Kubernetes Cluster Installation 

Please follow the steps carefully and read each command before executing.

### Create S3 bucket for storing the KOPS objects.

```
aws s3api create-bucket --bucket kops-abhi-storage --region us-east-1
```

### Create the cluster 

```
kops create cluster --name=demok8scluster.k8s.local --state=s3://kops-abhi-storage --zones=us-east-1a --node-count=1 --node-size=t2.micro --master-size=t2.micro  --master-volume-size=8 --node-volume-size=8
```

### Important: Edit the configuration as there are multiple resources created which won't fall into the free tier.

```
kops edit cluster myfirstcluster.k8s.local
```

Step 12: Build the cluster

```
kops update cluster demok8scluster.k8s.local --yes --state=s3://kops-abhi-storage
```

This will take a few minutes to create............

After a few mins, run the below command to verify the cluster installation.

```
kops validate cluster demok8scluster.k8s.local
```

## What is a POD?

In Kubernetes, a POD is a fundamental unit that encapsulates one or more containers. It serves as the smallest deployable entity, defining how containers should run within a shared context. Essentially, a POD is a wrapper around containers, and its behavior is described in a YAML file.

A POD can consist of a single container or multiple containers that work together. Each POD is assigned a Cluster IP, which allows communication within the cluster.

## Using kubectl: The Kubernetes Command Line

**kubectl** is the command-line interface used to interact with a Kubernetes cluster. It acts as the primary tool for managing and controlling various aspects of the cluster. With kubectl, you can perform operations such as creating, updating, and deleting resources.

## POD Deployment Demo

To illustrate POD deployment, follow these steps:

### 1. Install kubectl and Minikube (or kind).

- **Minikube:** A local Kubernetes cluster can be created using Minikube, which provides a simplified setup for testing and development.
  ```bash
  minikube start
  ```
  Ensure that virtualization is enabled and run the above command to create a single-node Kubernetes cluster.

- **kind (Kubernetes in Docker):** An alternative to Minikube is kind, which allows you to run Kubernetes clusters using Docker.
  ```bash
  kind create cluster
  ```
Certainly! A basic Kubernetes POD file is written in YAML format. Below is an example of a simple POD file that defines a single container:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mycontainer
    image: nginx
    ports:
    - containerPort: 80
```

Let's break down the components of this POD file:

- `apiVersion`: Specifies the version of the Kubernetes API to use.
- `kind`: Defines the type of Kubernetes object, in this case, a Pod.
- `metadata`: Contains metadata about the Pod, such as its name.
- `spec`: Describes the desired state for the Pod.
  - `containers`: An array of containers within the Pod.
    - `name`: A name for the container.
    - `image`: The container image to run.
    - `ports`: Defines ports to be exposed within the container.

In this example, the Pod is named "mypod," and it contains a single container named "mycontainer" based on the Nginx image. The container exposes port 80.

You can save this YAML content in a file, for example, `mypod.yaml`, and create the Pod using the following command:

### 2. Create a POD

Assuming you have a POD definition in a YAML file (pod.yml), use the following command to create the POD:
  ```bash
  kubectl create -f mypod.yml
  ```

### 3. Check POD Status

To verify the status of the deployed POD, use the following command:
  ```bash
  kubectl get pods
  ```
  For more detailed information:
  ```bash
  kubectl get pods -o wide
  ```

### 4. Log into Kubernetes Cluster

To log into the Kubernetes cluster created by Minikube, use the following command:
  ```bash
  minikube ssh
  ```
  Then, you can use tools like curl to interact with the assigned Cluster IP.

By following these steps, you've successfully demonstrated the deployment of a POD in a local Kubernetes cluster using Minikube or kind.

# Kubernetes Deployment and ReplicaSet - 

## What are the difference in Containers, Pods, and Deployments in Kubernetes

### Container:

A container is a lightweight, standalone, and executable software package that includes everything needed to run a piece of software, including the code, runtime, libraries, and system tools. Containers provide a consistent and reproducible environment, making it easy to deploy and scale applications across different environments.

### Pod:

A Pod is the smallest deployable unit in Kubernetes. It represents a single instance of a running process in a cluster and encapsulates one or more containers. Containers within a Pod share the same network namespace, allowing them to communicate with each other using `localhost`. Pods provide a way to deploy and manage tightly coupled applications or multiple containers that need to work together.

### Deployment:

A Deployment in Kubernetes is a higher-level abstraction for managing the deployment and scaling of applications. It allows you to declaratively define the desired state of your application, including the number of replicas (Pods) and the template for those Pods. Deployments ensure that the specified number of replicas are running and handle updates and rollbacks gracefully. They also provide features like scaling, rolling updates, and self-healing capabilities for applications.

## Leveraging Kubernetes Deployments for Autohealing and Auto Scaling

In Kubernetes, the **Deployment** resource plays a pivotal role in orchestrating the deployment, scaling, and management of applications. It is recommended to refrain from creating individual Pods directly; instead, use Deployments to achieve enhanced control and efficiency.

### Key Features:

#### Autohealing:

Kubernetes Deployments facilitate autohealing by continuously monitoring the state of the application and automatically responding to any disruptions. In the event of a Pod failure, the Deployment ensures the creation of a replacement Pod, maintaining the desired state and promoting application reliability.

#### Auto Scaling:

The Deployment resource allows you to effortlessly scale your application by defining the desired number of replicas. Kubernetes will automatically adjust the number of Pods based on the specified replica count, ensuring optimal resource utilization and responsiveness to varying workloads.

### Workflow:

1. **Define Desired State:**
   - Specify the application's desired state, including container images, resource requirements, and the number of replicas, in the Deployment configuration.

2. **Creation of Replica Set:**
   - When the Deployment is applied, Kubernetes creates a corresponding **Replica Set**. The Replica Set ensures that the specified number of Pods, as defined in the Deployment, is running at all times.

3. **Rolling Updates:**
   - Deployments support rolling updates, allowing for seamless updates to application versions without downtime. Kubernetes gradually replaces old Pods with new ones, ensuring a smooth transition.

Certainly! Here's an explanation of the architecture of the Deployment feature in Kubernetes for your GitHub documentation:

## Kubernetes Deployment Architecture

The **Deployment** feature in Kubernetes is a key component for managing the deployment and scaling of applications within a cluster. Understanding its architecture is crucial for effectively orchestrating application updates, rollbacks, and ensuring high availability.

### Components:

1. **Deployment Resource:**
   - At the heart of the architecture is the **Deployment resource**, a declarative configuration that defines the desired state of the application. It includes details such as container images, replica count, and update strategies.

2. **Controller Manager:**
   - The Kubernetes **Controller Manager** is responsible for maintaining the desired state specified in the Deployment resource. It continuously monitors the cluster and takes corrective actions to ensure the actual state matches the desired state.

3. **Replica Set:**
   - When a Deployment is created, it automatically generates a corresponding **Replica Set**. The Replica Set is responsible for managing a set of identical Pods, ensuring the desired number of replicas is running at all times.

4. **Pods:**
   - Pods are the smallest deployable units in Kubernetes and encapsulate one or more containers. The Deployment orchestrates the creation, scaling, and updating of Pods based on the specified configuration.

5. **Rolling Updates:**
   - Deployments support rolling updates, a key architectural feature. During a rolling update, the Deployment progressively replaces old Pods with new ones, ensuring zero downtime and continuous availability of the application.

### Workflow:

1. **Desired State Declaration:**
   - Users define the desired state of the application by creating or updating a Deployment resource, specifying details such as container images, replica count, and update strategies.

2. **Controller Manager Action:**
   - The Kubernetes Controller Manager continuously monitors the state of the cluster. When there's a deviation from the desired state (e.g., due to failures or changes), the Controller Manager takes corrective action to align the actual state with the desired state.

3. **Replica Set Management:**
   - The Replica Set, created by the Deployment, manages the lifecycle of Pods. It ensures the correct number of replicas are running and facilitates controlled updates.

4. **Rolling Updates Execution:**
   - During rolling updates, the Deployment coordinates the replacement of old Pods with new ones, maintaining application availability. This is achieved by gradually scaling up the new version while scaling down the old version.

Here's an example of a basic `deployment.yml` file for Kubernetes that you can include in your GitHub documentation. This example assumes a simple web application with a single container:

```yaml
# deployment.yml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-web-app-deployment
spec:
  replicas: 3  # Number of desired replicas (Pods)
  selector:
    matchLabels:
      app: my-web-app
  template:
    metadata:
      labels:
        app: my-web-app
    spec:
      containers:
      - name: my-web-app-container
        image: your-registry/your-web-app:latest
        ports:
        - containerPort: 80
```

Explanation:

- **apiVersion:** Specifies the API version for the Kubernetes resource. In this case, it's using the `apps/v1` version for Deployments.

- **kind:** Specifies the type of Kubernetes resource, which is a `Deployment` in this case.

- **metadata:** Provides metadata for the Deployment, including the name (`my-web-app-deployment`).

- **spec:** Describes the desired state of the Deployment.

  - **replicas:** Defines the desired number of replicas (Pods) to run, in this case, 3.

  - **selector:** Defines how the Deployment identifies which Pods to manage. In this case, it matches Pods with the label `app: my-web-app`.

  - **template:** Specifies the Pod template.

    - **metadata:** Provides labels for the Pods.

    - **spec:** Describes the container(s) to run in each Pod.

      - **containers:** Lists the containers in the Pod.

        - **name:** Specifies the name of the container.

        - **image:** Specifies the container image to use.

        - **ports:** Specifies the ports to expose on the container.

This is a basic example, and you may need to customize it according to your specific application requirements. 
