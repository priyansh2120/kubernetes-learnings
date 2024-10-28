# Kubernetes

## Container Orchestration

- **What are containers?** Isolated environments that contain everything an application needs to run. They are portable, efficient, and easy to use. Eg: Docker lets you create, deploy, and run containers.
- **Container Orchestration:** *Managing the lifecycle of containers*, especially in large, dynamic environments. It involves *deploying, scaling, and managing containers*.
- **Why do we need container orchestration?**
    - Starting up a single container requires a lot of manual work. Imagine managing a **cluster** of VMs containing multiple containers.
    - If we want to manage these clusters for us, like simply provide the scripts/instructions to someone else, and it will take care of the rest.
    - If we want to move our containers from one host to another, we need to make sure that the containers are running and the data is not lost.
    - Along with the above points, what if we can monitor the health of our containers, and if they are unhealthy, we can restart them automatically. This is also where container orchestration comes in.

## Different Architectures for deploying before K8s

- Load Balancer -> Multiple Servers (For Backend) [Right way for large applications]
    
    https://github.com/its-id/100x-Cohort-Programs/assets/60315832/1694ffd7-1d95-4c65-a321-fe08625d620e
    
- EC2 Machine (For Frontend)
    
    https://github.com/its-id/100x-Cohort-Programs/assets/60315832/0461097b-9daf-4b76-937c-7e583c510770
    
- CDN -> S3 Bucket (For Static Files)
    
    https://github.com/its-id/100x-Cohort-Programs/assets/60315832/8876ac42-0edc-4b9b-a7d9-4d7f7166623e
    

## Deploying Architecture with K8s

- In Kubernetes, if we want to start a container we don't start a container directly. We start a **Pod**.
    
    ***Note**: A single **Pod** can contain one or more containers.*
    
- A **Node** is simply an EC2 machine (VM) where we can run our containers. In Kubernetes, we divide the **Nodes** into two categories:
    - Master Nodes (Control Pane)
    - Worker Nodes
- **Master Nodes** are responsible for managing the **Pods**. It's the master node's responsibility to:
    - deploy bunch of containers as per the requirement.
    - manage them.
- **Worker Nodes** are the ones who actually run the containers.
- **Kubernetes Cluster** is the collection of **Master Nodes** and **Worker Nodes**.

<details>
<summary> <b>Master Node Internals</b> </summary>

https://github.com/its-id/100x-Cohort-Programs/assets/60315832/5d57f53b-d892-4d96-8f25-bf8736152f78

- **API Server**: The main entry point for all requests. Developers send requests (e.g., 'pls initiate and run that docker image') to this server inside master nodes.
- **etcd**: A distributed key-value store that stores the cluster state (e.g., pod-to-container mappings).
- **Scheduler**: Decides where to run the container based on the cluster state.
- **Controller Manager**: Ensures the cluster state aligns with requirements, managing container restarts if necessary.

### Worker Node Internals

https://github.com/its-id/100x-Cohort-Programs/assets/60315832/4ef57927-0998-4106-a1b5-58ec4995b6ed

- **container runtime**: Where the container actually runs, e.g., Docker.
- **kubelet**: An agent that ensures containers are running in a pod on each worker node.
- **kube-proxy**: Ensures proper network setup, enabling inter-pod communication.

- Summary hierarchy 👇
    
    https://github.com/its-id/100x-Cohort-Programs/assets/60315832/ee9aef4f-51ef-455c-b5ef-fc394da358c6
    

## Creating a Kubernetes Cluster

Two ways to create a Kubernetes cluster:

**Locally**:

- Minikube
- Kind (Recommended)

**Cloud**:

- GKE (Google Kubernetes Engine)

### Installing & Setting up a Cluster

1. Install Kind using instructions [here](https://kind.sigs.k8s.io/docs/user/quick-start/).
2. Create a single-node cluster:
    
    ```bash
    kind create cluster --name 100x-k8s
    
    ```
    
3. Check Docker containers running:
    
    ```bash
    docker ps
    
    ```
    
4. To delete the cluster:
    
    ```bash
    kind delete cluster --name 100x-k8s
    
    ```
    
5. For a multi-node cluster, create a `clusters.yml` file and run:
    
    ```bash
    kind create cluster --config clusters.yml --name 100x-k8s
    
    ```
    
6. To check cluster info (may give forbidden error):
    
    ```bash
    kubectl cluster-info --context kind-100x-k8s
    
    ```
    
7. To install `kubectl` and check installation:
    
    ```bash
    brew install kubectl
    kubectl version
    
    ```
    
8. Access the cluster using `kubectl`:
    
    ```bash
    kubectl get nodes
    
    ```
    

---

## Creating a Pod

1. Check running pods (should show none):
    
    ```bash
    kubectl get pods
    
    ```
    
2. To create an Nginx container inside a pod:
    
    ```bash
    kubectl run nginx --image=nginx --port=80
    
    ```
    
3. To delete the pod:
    
    ```bash
    kubectl delete pod nginx
    
    ```
    

### Creating a Pod using Manifest File

1. Create a `pod-manifest.yml` file with the following:
    
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
          ports:
            - containerPort: 80
    
    ```
    
2. Create the pod:
    
    ```bash
    kubectl apply -f pod-manifest.yml
    
    ```
    
3. Delete the pod:
    
    ```bash
    kubectl delete pod nginx
    
    ```
    

### Checkpoint

Current state of cluster 👇

https://github.com/its-id/100x-Cohort-Programs/assets/60315832/02768715-7125-41ad-bb70-3d31f3f8dad2
