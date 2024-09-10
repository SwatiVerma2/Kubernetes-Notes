# Kubernetes – Cluster Architecture

- Kubernetes comes with a client-server architecture. 
- It consists of **master and worker nodes**, with the master being installed on a single Linux system and the nodes on many Linux workstations. 
- The master node, contains the components such as API Server, controller manager, scheduler, and etcd database for stage storage. kubelet to communicate with the master, the kube-proxy for networking, and a container runtime such as Docker to manage containers.
  
![image](https://github.com/user-attachments/assets/f7368e64-b0f7-402e-8615-49480bd01809)

# Kubernetes Components
Kubernetes components can be divided into two categories:

1. Worker nodes / minion: Each Kubernetes cluster requires at least one worker node, which is a collection of worker machines that make up the nodes where our container will be deployed.
2. Control plane / Master plane: The worker nodes and any pods contained within them will be under the control plane.


## Control Plane Components

**1. kube-apiserver:**
- Acts as the front-end for the Kubernetes control plane. It exposes the Kubernetes API, which is the central point for managing the cluster.
- All requests from users, external components, or internal components go through the API server.
- It validates and processes API requests, then updates the cluster’s state.
  
**2. etcd:**
- A consistent and highly available key-value store used as Kubernetes' backing store for all cluster data.
- Stores configuration data, cluster state, and any changes made by the control plane.
- It is the source of truth for the cluster and is critical for Kubernetes’ functioning.
  
**3. kube-scheduler: (action)**
- Determines on which node a newly created pod will run, based on factors like resource availability, workload requirements, and policies.
- It considers constraints such as CPU, memory, or node affinity when making scheduling decisions.

**4. kube-controller-manager:**
- Runs controllers, which are background processes that ensure the desired state of the cluster matches the current state.
- Example controllers include:
   --> Node Controller: Ensures all nodes in the cluster are healthy.
  
   --> Replication Controller: Ensures the correct number of pod replicas are running.
  
   --> Endpoint Controller: Manages service endpoints.

# Components of Worker Node

The Components of a Worker Node in Kubernetes are responsible for running the actual workloads (containers) and maintaining the node's operation. Here’s a brief explanation of each:

**1. kubelet:**
- The primary agent that runs on each worker node.
- It ensures that containers are running in pods as defined by the control plane.
- The kubelet continuously monitors the health of the pods and communicates with the control plane to report back the node’s status.
  
**2. Container Runtime:**
- The software responsible for running containers on the worker node. Common runtimes include Docker, containerd, and CRI-O.
- It pulls container images, creates, manages, and runs containers as per the instructions from Kubernetes.
  
**3. kube-proxy:**
- A network component that runs on each worker node.
- It maintains network rules on the node to allow for communication between different pods (inside and outside the cluster).
- It forwards traffic between pods and services based on the cluster's service and pod IPs.


# Pods
## What is a Pod in Kubernetes?
- A Pod is the smallest, most basic unit of deployment in Kubernetes. 
- It represents a single instance of a running process in a cluster. 
- Each pod can contain one or more tightly coupled containers that share the same network namespace, IP address, and storage volumes. 
- Pods are **ephemeral**, meaning they can be created, destroyed, and replaced as needed.

## Limitations of a Pod:

**1. Ephemeral Nature:** Pods are designed to be temporary and can be destroyed and recreated by Kubernetes. If a pod dies, it won't automatically restart unless managed by a higher-level controller.

**2. Lack of Scalability:** A pod is a single instance of an application. Managing and scaling multiple pods manually across nodes is complex without automation.

**3. No Built-in Auto-Healing:** Pods do not automatically self-heal. If they fail, Kubernetes won’t restart them on its own unless they are part of a higher-level object.

**4. Static Assignment:** Pods don't support dynamic placement or scaling across multiple nodes without the use of controllers.

### Objects that Overcome Pod Limitations:

- ReplicaSet:
- Deployment:
- StatefulSet:
- DaemonSet:
- Job/CronJob:
- Horizontal Pod Autoscaler (HPA):
