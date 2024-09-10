# Basic Commands

1. **`minikube start --driver=docker`**:
   - This command starts a **Minikube** Kubernetes cluster using **Docker** as the container runtime driver. Minikube is a tool to run Kubernetes locally, and specifying the `--driver=docker` tells it to use Docker containers instead of creating a virtual machine.

2. **`minikube status`**:
   - This shows the status of your Minikube cluster. It gives details like whether the cluster is running, the status of the host, Kubernetes components, and the kubelet.

3. **`kubectl get nodes`**:
   - This command lists all nodes in your Kubernetes cluster. Nodes are the physical or virtual machines (worker nodes) where your Kubernetes pods run. This shows the node names, status, and other details.

4. **`kubectl describe node <node-name>`**:
   - Provides detailed information about a specific node. It shows information such as resource usage (CPU, memory), conditions (health of the node), and the running pods on the node. Replace `<node-name>` with the name of the node you want to inspect.

5. **`kubectl apply -f <yml-file>`**:
   - This applies or updates a Kubernetes configuration using the provided YAML file (`<yml-file>`). The file typically defines resources like deployments, services, or config maps. Kubernetes will create or update the resources based on this file.

6. **`kubectl get pods`**:
   - Lists all pods running in the current namespace. A pod is the smallest deployable unit in Kubernetes and usually contains one or more containers. The command shows pod names, status, and other basic details.

7. **`kubectl get pods -o wide`**:
   - This is an extended version of `kubectl get pods`. It shows additional information such as the node on which each pod is running, IP addresses, and the container image used.

8. **`kubectl describe pod <pod-name>` / `kubectl describe pod/<pod-name>`**:
   - Provides detailed information about a specific pod. It shows events, container states, volumes, and any errors that occurred. It’s useful for troubleshooting. Replace `<pod-name>` with the name of the pod you want to describe.

9. **`kubectl logs -f <pod-name>` or `kubectl logs -f <pod-name> -c <container-name>`**:
   - `kubectl logs -f <pod-name>`: Streams the logs of a pod in real-time, useful for monitoring live output.
   - `kubectl logs -f <pod-name> -c <container-name>`: Streams logs for a specific container inside a pod if the pod has multiple containers.

10. **`kubectl delete pod <pod-name>` or `kubectl delete -f <yml-name>`**:
    - `kubectl delete pod <pod-name>`: Deletes a specific pod by name. The pod will be terminated and eventually replaced if it’s part of a deployment.
    - `kubectl delete -f <yml-name>`: Deletes all the resources defined in the specified YAML file.

