# Day-50-Kubernetes-Architecture-and-Cluster-Setup

 ## Task 1: Recall the Kubernetes Story
 
Before touching a terminal, write down from memory:

  1. Why was Kubernetes created? What problem does it solve that Docker alone cannot?
     - Kubernetes was created to manage and automate containerized applications at scale. Docker can run containers, but it cannot easily handle multiple containers across many machines. Kubernetes solves problems like container orchestration, scaling, load balancing, self-healing (restarting failed containers), and automated deployment.
       
  2. Who created Kubernetes and what was it inspired by?
     - Kubernetes was originally created by Google. It was inspired by Google’s internal system called Borg, which they used for managing large-scale applications.
       
  3. What does the name "Kubernetes" mean?
     - Kubernetes comes from a Greek word meaning “helmsman” or “ship pilot”, which reflects its role in steering and managing containerized applications.

## Task 2: Draw the Kubernetes Architecture

![Image](https://github.com/user-attachments/assets/ee217271-dcd5-435e-8dc0-73ab675f9dba)

## Control Plane (Master Node):

API Server — the front door to the cluster, every command goes through it

etcd — the database that stores all cluster state

Scheduler — decides which node a new pod should run on

Controller Manager — watches the cluster and makes sure the desired state matches reality

## Worker Node:

kubelet — the agent on each node that talks to the API server and manages pods

kube-proxy — handles networking rules so pods can communicate

Container Runtime — the engine that actually runs containers (containerd, CRI-O)

1. What happens when you run kubectl apply -f pod.yaml? Trace the request through each component.
   - kubectl sends request → API Server
   - API Server validates & stores data in etcd
   - Controller Manager sees new pod (desired state)
   - Scheduler selects best worker node
   - API Server updates pod assignment
   - kubelet on that node gets instructions
   - kubelet asks Container Runtime to create container
   - Pod starts running
    
2. What happens if the API server goes down?
   -Cluster becomes unmanageable 
   - No new deployments, scaling, or updates
   - Existing running pods continue working (no immediate crash)
   - But no control → system is “blind”
     
3. What happens if a worker node goes down?
   - Pods on that node become unavailable 
   - Controller Manager detects failure
   - New pods are created on other healthy nodes 
   - This is called self-healing
  
## Task 3: Install kubectl

kubectl is the CLI tool you will use to talk to your Kubernetes cluster.

# Linux (amd64)
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

Verify:

kubectl version --client

<img width="984" height="443" alt="Image" src="https://github.com/user-attachments/assets/90761c85-4fcf-4ab2-b7e1-16ca370aef11" />

## Task 4: Set Up Your Local Cluster

kind (Kubernetes in Docker)

# Linux
curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

# Create a cluster
kind create cluster --name devops-cluster

<img width="1189" height="354" alt="Image" src="https://github.com/user-attachments/assets/3958656e-c67e-4812-93e7-df6b61d616e2" />

# Verify
kubectl cluster-info

kubectl get nodes

<img width="1176" height="203" alt="Image" src="https://github.com/user-attachments/assets/e127e59b-50e3-4294-8c1a-a78453b03cfa" />

## Task 5: Explore Your Cluster

Now that your cluster is running, explore it

kubectl cluster-info:

<img width="1243" height="113" alt="Image" src="https://github.com/user-attachments/assets/c1e3138d-de44-4a46-a6dc-0fc1f4d732f9" />

kubectl get nodes:

<img width="955" height="66" alt="Image" src="https://github.com/user-attachments/assets/b3486e09-f53c-4df6-8880-93741d5690ce" />

## Task 6: Practice Cluster Lifecycle

Build muscle memory with cluster operations:

kind delete cluster --name devops-cluster:

<img width="802" height="65" alt="Image" src="https://github.com/user-attachments/assets/52687166-1308-4882-be5c-8ce8e47894b8" />


kind create cluster --name devops-cluster:

<img width="1348" height="311" alt="Image" src="https://github.com/user-attachments/assets/b3f30a86-9244-485f-b2ec-0ab9b10294fe" />

kubectl get nodes:

<img width="955" height="66" alt="Image" src="https://github.com/user-attachments/assets/b645bbe7-8d54-4037-842f-ab282cdf6fe3" />

kubectl config current-context:

<img width="669" height="45" alt="Image" src="https://github.com/user-attachments/assets/3e65423c-ef99-4672-acb5-89e2f9dbbcf3" />


kubectl config get-contexts:

<img width="1346" height="91" alt="Image" src="https://github.com/user-attachments/assets/c74cf308-7d15-4d39-b9df-930b05dc9de4" />

kubectl config view:

<img width="656" height="699" alt="Image" src="https://github.com/user-attachments/assets/ab6a4b52-bd09-4dc7-b3b9-007939914156" />







 




