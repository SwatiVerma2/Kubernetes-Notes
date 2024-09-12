# Basic Commands

1. **`minikube start --driver=docker`**:
   - This command starts a **Minikube** Kubernetes cluster using **Docker** as the container runtime driver. Minikube is a tool to run Kubernetes locally, and specifying the `--driver=docker` tells it to use Docker containers instead of creating a virtual machine.

2. **`minikube status`**:
   - This shows the status of your Minikube cluster. It gives details like whether the cluster is running, the status of the host, Kubernetes components, and the kubelet.

   ![image](https://github.com/user-attachments/assets/dda33c11-3dc8-4bba-aaac-21f546e32eae)

3. **`kubectl get nodes`**:
   - This command lists all nodes in your Kubernetes cluster. Nodes are the physical or virtual machines (worker nodes) where your Kubernetes pods run. This shows the node names, status, and other details.

   ![image](https://github.com/user-attachments/assets/1b54d3e6-f363-409f-ba4c-ae49c3d08dd3)

4. **`kubectl describe node <node-name>`**:
   - Provides detailed information about a specific node. It shows information such as resource usage (CPU, memory), conditions (health of the node), and the running pods on the node. 

   ![image](https://github.com/user-attachments/assets/f6b742b1-7f3b-4506-a68b-a09fd16ec266)

5. **`kubectl apply -f <yml-file>`**:
   - This applies or updates a Kubernetes configuration using the provided YAML file (`<yml-file>`). The file typically defines resources like deployments, services, or config maps. Kubernetes will create or update the resources based on this file.

**yaml file**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myfirstpod
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

   ![image](https://github.com/user-attachments/assets/6fc9c14a-88f0-4ad7-96b7-268ddb3c2b3c)

6. **`kubectl get pods`**:
   - Lists all pods running in the current namespace. The command shows pod names, status, and other basic details.

      ![image](https://github.com/user-attachments/assets/28429a1f-5b04-49e0-9b8a-24a116130523)

7. **`kubectl get pods -o wide`**:
   - It shows additional information such as the node on which each pod is running, IP addresses, and the container image used.

      ![image](https://github.com/user-attachments/assets/9cbfaafa-526b-4827-927f-ec9439112292)

8. **`kubectl describe pod <pod-name>` / `kubectl describe pod/<pod-name>`**:
   - Provides detailed information about a specific pod. It shows events, container states, volumes, and any errors that occurred. It’s useful for troubleshooting. 

      ![image](https://github.com/user-attachments/assets/a7279272-d05b-4876-9159-c13a5784d07d)

9. **`kubectl logs -f <pod-name>` or `kubectl logs -f <pod-name> -c <container-name>`**:
   - `kubectl logs -f <pod-name>`: Streams the logs of a pod in real-time, useful for monitoring live output.
     
      ![image](https://github.com/user-attachments/assets/68a1ea9e-0459-4fe6-9c2d-0433d686e78a)
     
   - `kubectl logs -f <pod-name> -c <container-name>`: Streams logs for a specific container inside a pod if the pod has multiple containers.
     
      ![image](https://github.com/user-attachments/assets/ded2d28b-b7b3-44f5-9785-e02295805d4f)

  
10. **`kubectl delete pod <pod-name>` or `kubectl delete -f <yml-name>`**:
    - `kubectl delete pod <pod-name>`: Deletes a specific pod by name. The pod will be terminated and eventually replaced if it’s part of a deployment.
      
      ![image](https://github.com/user-attachments/assets/c8b398a5-a717-4c02-b2e5-94591bb9a35f)

    - `kubectl delete -f <yml-name>`: Deletes all the resources defined in the specified YAML file.
   
