# Kubernetes – Cluster Architecture

- Kubernetes comes with a client-server architecture. 
- It consists of **master and worker nodes**, with the master being installed on a single Linux system and the nodes on many Linux workstations. 
- The master node, contains the components such as API Server, controller manager, scheduler, and etcd database for stage storage. kubelet to communicate with the master, the kube-proxy for networking, and a container runtime such as Docker to manage containers.
  
![image](https://github.com/user-attachments/assets/f7368e64-b0f7-402e-8615-49480bd01809)

## Kubernetes Components
Kubernetes components can be divided into two categories:

1. Worker nodes / minion: Each Kubernetes cluster requires at least one worker node, which is a collection of worker machines that make up the nodes where our container will be deployed.
2. Control plane / Master plane: The worker nodes and any pods contained within them will be under the control plane.


### Control Plane Components

1. kube-apiserver:
- Acts as the front-end for the Kubernetes control plane. It exposes the Kubernetes API, which is the central point for managing the cluster.
- All requests from users, external components, or internal components go through the API server.
- It validates and processes API requests, then updates the cluster’s state.
  
2. etcd:
- A consistent and highly available key-value store used as Kubernetes' backing store for all cluster data.
- Stores configuration data, cluster state, and any changes made by the control plane.
- It is the source of truth for the cluster and is critical for Kubernetes’ functioning.
  
3. kube-scheduler: (action)
- Determines on which node a newly created pod will run, based on factors like resource availability, workload requirements, and policies.
- It considers constraints such as CPU, memory, or node affinity when making scheduling decisions.

4. kube-controller-manager:
- Runs controllers, which are background processes that ensure the desired state of the cluster matches the current state.
- Example controllers include:
  
    Node Controller: Ensures all nodes in the cluster are healthy.
  
    Replication Controller: Ensures the correct number of pod replicas are running.
  
    Endpoint Controller: Manages service endpoints.
