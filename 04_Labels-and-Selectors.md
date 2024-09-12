# Labels and Selectors

- **Labels** and **Selectors** are key concepts used to organize, manage, and filter resources like Pods, Nodes, Services, etc. 
- They help Kubernetes efficiently identify and manage groups of objects, especially when dealing with dynamic environments.

### 1. **Labels in Kubernetes**
   - **Definition**: Labels are key-value pairs attached to Kubernetes objects such as Pods, Nodes, and Services. They are used to organize and categorize resources within a cluster.
   - **Purpose**: Labels are designed to allow users to identify and select resources easily. They do not have any implicit semantic meaning to the system itself, meaning Kubernetes doesn’t act on labels by default unless they are used in selectors.
   
   **Example of labels**:
   ```yaml
apiVersion: v1
kind: Pod
metadata:
  name: firstpod
  labels:
    organisation: nvidia
    environment: production
spec:
  containers:
  - name: c1
    image: ubuntu
    ports:
    - containerPort: 80

   ```

   In this example, the Pod is labeled with `app: frontend` and `environment: production`.

### Command

- `kubectl get pods --show-labels` : lists all the Pods in the current namespace and displays their associated labels. It's a useful way to see the labels that have been applied to your Pods.

  ![image](https://github.com/user-attachments/assets/5eb713bb-6989-40e5-a4a4-1c010b316c4c)


- `kubectl label <resource-type> <resource-name> <key>=<value>`: This command adds or modifies a label for a specific resource (like a Pod, Node, or Service).
  
  Eg. `kubectl label pod my-pod app=frontend`

- `kubectl get pods -l <key>=<value>` : retrieves all Pods that have a label matching the specified key-value pair. This is useful for filtering resources based on labels.
  
![image](https://github.com/user-attachments/assets/63aea79c-3263-44b0-bab7-9a8622891044)

### 2. **Selectors in Kubernetes**
   - **Definition**: Selectors are used to **filter** and identify objects based on their labels. They are mainly used by Kubernetes resources like Services, ReplicaSets, or Deployments to select specific Pods.
   - **Purpose**: Selectors enable you to query and filter resources based on the labels you have assigned to them. This allows you to target groups of resources dynamically without having to explicitly name them.
     
### Types of Selectors
     
1. **Equality-based Selectors**: Select objects that have a specific label key-value pair. Eg. =, !=
       
   Eg1. kubectl get pods -l environment!=production
       
    ![image](https://github.com/user-attachments/assets/3211819f-e239-4585-a210-913b56c4a814)

   Eg2. kubectl get pods -l environment=production
     
   ![image](https://github.com/user-attachments/assets/35240b27-75d3-406e-865f-5bf8548437e6)

2. **Set-based Selectors**: Select objects based on a set of conditions like `in`, `notin`, `exists`.

   Eg. kubectl get pods -l 'organisation in (nvidia)'
     
     ![image](https://github.com/user-attachments/assets/e961968c-a45d-473f-a2c5-796e0bfc1d08)

 
### **How Labels and Selectors Work Together**
   - **Labels**: Attach metadata to objects.
   - **Selectors**: Enable resources (like Services or Deployments) to select and operate on a group of objects based on their labels.

### **Common Use Cases**:
1. **Service Discovery**: A Service can use selectors to automatically route traffic to Pods that have a matching label (e.g., `app: frontend`).
2. **Scaling**: A Deployment can use labels to identify the Pods it needs to scale.
3. **Managing Environments**: Labels can help distinguish resources for different environments like `environment: dev` or `environment: production`.

# Node Selector

- A Node Selector in Kubernetes is a mechanism that allows you to control which Nodes a Pod is scheduled on, based on labels applied to the Nodes. 
- It ensures that Pods are scheduled only on Nodes that match specific criteria. 
- This is particularly useful when you want to run certain workloads on specific types of hardware, environments, or zones within your cluster.

### **How Node Selector Works:**
- Nodes are labeled: Nodes in a Kubernetes cluster are tagged with labels (key-value pairs) that define their characteristics. For example, a Node could be labeled as disktype=ssd or environment=production.
- Pods specify a Node Selector: In the Pod's specification, you can add a nodeSelector field with a key-value pair that matches a Node's label. Kubernetes will only schedule that Pod on a Node that has the specified label.

### Example of a Node Selector

1. Labeling a Node: `kubectl label node <node-name> disktype=ssd`

2. Using nodeSelector in a Pod Spec: you specify the nodeSelector field to ensure the Pod is only scheduled on Nodes that have the disktype=ssd label.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: nginx
  nodeSelector:
    disktype: ssd
```
- the Pod my-pod will only be scheduled on a Node that has the label disktype=ssd.
