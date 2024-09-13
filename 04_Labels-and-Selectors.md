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
    command: ["sleep", "infinity"]
    ports:
    - containerPort: 80

   ```

   In this example, the Pod is labeled with `organisation: nvidia` and `environment: production`.

### Command
1. `kubectl get pods --show-labels` : lists all the Pods in the current namespace and displays their associated labels. It's a useful way to see the labels that have been applied to your Pods.

   ![image](https://github.com/user-attachments/assets/9941940e-b64f-4412-b18a-47f470c5dd16)

2. `kubectl label <resource-type> <resource-name> <key>=<value>`: This command adds or modifies a label for a specific resource (like a Pod, Node, or Service).
  
    Eg. `kubectl label pod firstpod name=swati`
         
     ![image](https://github.com/user-attachments/assets/10f365fc-af9a-4743-86c3-a12d911af77f)

4. `kubectl get pods -l <key>=<value>` : retrieves all Pods that have a label matching the specified key-value pair. This is useful for filtering resources based on labels.
  
   ![image](https://github.com/user-attachments/assets/3ca1a84b-08d3-4274-9750-0ef6a1cc24ea)

### 2. **Selectors in Kubernetes**
   - **Definition**: Selectors are used to **filter** and identify objects based on their labels. They are mainly used by Kubernetes resources like Services, ReplicaSets, or Deployments to select specific Pods.
   - **Purpose**: Selectors enable you to query and filter resources based on the labels you have assigned to them. This allows you to target groups of resources dynamically without having to explicitly name them.
     
### Types of Selectors
     
1. **Equality-based Selectors**: Select objects that have a specific label key-value pair. Eg. =, !=
       
   Eg 1. kubectl get pods -l environment!=production
       
    ![image](https://github.com/user-attachments/assets/3211819f-e239-4585-a210-913b56c4a814)

   Eg 2. kubectl get pods -l environment=production
     
     ![image](https://github.com/user-attachments/assets/4cdddc42-ba0c-40d1-b7bf-8a367567f253)

2. **Set-based Selectors**: Select objects based on a set of conditions like `in`, `notin`, `exists`.

   Eg 1.  kubectl get pods -l 'environment in (production,staging)'
     
     ![image](https://github.com/user-attachments/assets/ddf2a852-a401-46bb-8fef-ce2a8e5a0cb2)

   Eg 2. kubectl get pods -l 'environment notin (production)'
   
   ![image](https://github.com/user-attachments/assets/0ebfd91d-3f05-40b8-9155-113b6c54fdf0)
 
### **How Labels and Selectors Work Together**
   - **Labels**: Attach metadata to objects.
   - **Selectors**: Enable resources (like Services or Deployments) to select and operate on a group of objects based on their labels.

### **Common Use Cases**:
1. **Service Discovery**: A Service can use selectors to automatically route traffic to Pods that have a matching label (e.g., `app: frontend`).
2. **Scaling**: A Deployment can use labels to identify the Pods it needs to scale.
3. **Managing Environments**: Labels can help distinguish resources for different environments like `environment: dev` or `environment: production`.

# Node Selector

- A Node Selector is a mechanism that allows you to control which Nodes a Pod is scheduled on, based on labels applied to the Nodes. 
- It ensures that Pods are scheduled only on Nodes that match specific criteria. 
- This is particularly useful when you want to run certain workloads on specific types of hardware, environments, or zones within your cluster.

### **How Node Selector Works:**
- Nodes are labeled: Nodes in a Kubernetes cluster are tagged with labels that define their characteristics.
- Pods specify a Node Selector: In the Pod's specification, you can add a nodeSelector field with a key-value pair that matches a Node's label. Kubernetes will only schedule that Pod on a Node that has the specified label.

### Example of a Node Selector

1. Labeling a Node:  `kubectl label node minikube instance=t2.micro`

2. Using nodeSelector in a Pod Spec: you specify the nodeSelector field to ensure the Pod is only scheduled on Nodes that have the disktype=ssd label.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: node-selector
spec:
  containers:
    - name: mycontaier
      image: ubuntu
      command: ["sleep", "infinity"] # Keeps the container alive
  nodeSelector:
    instance: t2.micro

```
- the Pod node-selector will only be scheduled on a Node that has the label instance=t2.micro.
  
  ![image](https://github.com/user-attachments/assets/c8295333-fff0-4667-9531-fdc5d6fa7573)

- if  no node is avaible with that label it's gonna create an error. `kubectl describe pod nodeselector`
  
  ![image](https://github.com/user-attachments/assets/9512d9cc-5297-4b75-940e-f4509c36af9e)

