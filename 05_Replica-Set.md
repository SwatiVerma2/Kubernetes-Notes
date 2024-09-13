# Scaling and Replication

## Scaling
Scaling refers to adjusting the number of replicas of an application (Pods) running in a Kubernetes cluster. It can be either horizontal scaling (adding/removing Pods) or vertical scaling (changing resource limits, such as CPU and memory, allocated to Pods).

### Types of Scaling:

1. Horizontal Scaling (the most common in Kubernetes)
   
   - Scaling Out: Adding more Pods to distribute load.
   - Scaling In: Reducing the number of Pods when the load decreases.
   - Example: To scale a Deployment to 5 replicas:
   - `kubectl scale deployment <deployment-name> --replicas=5`
   - Kubernetes also offers auto-scaling using the **Horizontal Pod Autoscaler (HPA)**, which adjusts the number of replicas based on CPU utilization or other custom metrics.

2. Vertical Scaling

   - Adjusting the amount of CPU or memory allocated to a Pod, rather than the number of Pods. 
   - This can be done manually by modifying resource requests/limits in the Pod specification, but it's less common than horizontal scaling in Kubernetes due to the ease and flexibility of scaling Pods.

## Relication

- Replication refers to running multiple instances (replicas) of a Pod to ensure high availability, fault tolerance, and load distribution. 
- If one Pod fails, others remain available to continue handling traffic.
- **Replication Controller and ReplicaSet** are Kubernetes objects used to manage replication:
- ReplicaSet is the modern equivalent of the Replication Controller and is responsible for maintaining the desired number of Pod replicas.

### Difference between ReplicaSet and ReplicationController

- The main difference between ReplicaSet and ReplicationController is that ReplicaSet allows for more powerful queries through set-based selectors.
- These selectors enable conditions like "in", "notin", or "exists", providing more flexibility when matching Pods based on labels.
- ReplicationController only supports equality-based selectors (e.g., key=value), making ReplicaSet more versatile in modern Kubernetes environments.

# Replica Set

- A ReplicaSet in Kubernetes ensures that a specified number of pod replicas (identical pods) are running at any given time. 
- It monitors the pods and automatically replaces any that fail or are deleted to maintain the desired number of replicas.

### Features

- Self-Healing: Automatically replaces failed or deleted pods.
- Scaling: Adjusts the number of pod replicas up or down.
- Label-Based Management: Uses labels to select and manage pods.
- Declarative Updates: Ensures the desired state of pods is maintained.

### Example

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replica-set
  labels:
    app: guestbook
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:                       
      containers:                 
        - name: mycontainer       
          image: ubuntu
          command: ["sleep", "infinity"] 
```

3 replicas are created

![image](https://github.com/user-attachments/assets/1d07bc49-f4ba-4a71-a79f-bd5afaef2f8c)

Even if you delete these pods it will be created automatically [auto healing]

![image](https://github.com/user-attachments/assets/ef11886d-cdce-4c98-b3ba-78ece0d8178c)

To delete them use `kubectl delete -f replicaset.yml`

# Replication Controller

- A Replication Controller in Kubernetes is an older resource used to ensure that a specified number of pod replicas are running at any given time. 
- It ensures high availability and fault tolerance for applications.

### Features

- Maintains Replica Count: Keeps a specified number of pods running, replacing any that fail.
- Label Selector: Manages pods based on labels.
- Pod Management: Handles starting, stopping, and replacing pods.
- Deprecated: Replaced by ReplicaSets and Deployments for better functionality and management.
