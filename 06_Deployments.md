# Deployments
- Deployment is a higher-level abstraction used to **manage the lifecycle of Pods and ReplicaSets.**
- It creates and manages Pods, through a ReplicaSet, and ensures the desired state of your application is maintained.
- Deployment -> Replica Set -> Pod
- It provides a more powerful and flexible way to handle rolling updates, scaling, rollbacks, and self-healing of your application.

### Features 

1. **Declarative Updates:** Define the desired state (e.g., replicas, container images) and Kubernetes ensures the current state matches this.

2. **Rolling Updates:** Gradually replace old Pods with new versions during updates, ensuring zero downtime.

3. **Rollback:** Easily revert to a previous version if an update fails.

4. **Scaling:** Adjust the number of replicas manually or automatically with a Horizontal Pod Autoscaler.

5. **Self-healing:** Automatically replaces failed or unresponsive Pods to maintain the desired replica count.

6. **Versioning and History:** Keeps a history of updates, allowing you to review and roll back to previous states if needed.

### Use Cases:

- Automated Updates: When updating an application, Deployments make sure the update is applied smoothly without causing service disruption.
- Scaling: Easily scale your application based on demand.
- Version Control and Rollbacks: Maintain control over application versions, allowing you to revert to a stable version if an update fails.

## Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mydeployment
  labels:
    org: nvidia
spec:
  replicas: 3
  selector:
    matchLabels:
      org: nvidia
  template:
    metadata:
      labels:
        org: nvidia
    spec:
      containers:                 
        - name: mycontainer       
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo Karan Aujla; sleep 5; done"]
```

### Commands

- `kubectl get deploy`
  
   ![image](https://github.com/user-attachments/assets/1bfe93ed-5dcb-4b0f-957e-e682af9fe64f)

- `kubectl describe deploy <deployment-name>`
  
- `kubectl get rs`
  
  ![image](https://github.com/user-attachments/assets/a34b66df-68a9-4b49-9af0-97b03c5111c8)

- `kubectl scale --replicas=1 deploy <deployment-name>`
  
    ![image](https://github.com/user-attachments/assets/716e2e40-18e3-4e73-a56f-a63cd5f170cb)
  
- `kubectl logs -f mydeployment-9669f5597-fl84s`

![image](https://github.com/user-attachments/assets/fd685935-5dea-4808-99e6-d67aba3a80e2)

#### Make some chnages in the yaml file

- Change the image to `centos` and echo comomand:
  
  ```yaml
  containers:                 
        - name: mycontainer       
          image: centos
          command: ["/bin/bash", "-c", "while true; do echo Diljit Dosanjh; sleep 5; done"]
  ```
  
- A new replica set will be created -> **Versioning**
  
  ![image](https://github.com/user-attachments/assets/5b1f6ff8-e39e-4d67-832c-65461a554369)

- logs: `kubectl logs -f mydeployment-5d7b9d46c-5hmn7`
  
  ![image](https://github.com/user-attachments/assets/a19c8d9d-6254-467f-81a2-55197c99a9b8)

- You can also check the OS:  `kubectl exec mydeployment-5d7b9d46c-5hmn7 -- cat /etc/os-release`
  
  ![image](https://github.com/user-attachments/assets/f00c070c-a0c5-4804-9ff3-8c740ed98a54)

- `kubectl rollout status deployment <deployment-name>` 

  - This command monitors the status of a rollout for a specific Deployment.
  - It provides real-time information about the progress of a Deployment update.
  - This command tells you whether the Deployment is in progress, successful, or encountering issues.
  - When to Use:
      1. Monitoring Progress: To check if a new version of the application has been deployed successfully or if there are issues during the update.
      2. Verification: After initiating an update or rollback, use this command to confirm that the process has completed successfully.
         
     ![image](https://github.com/user-attachments/assets/c3827159-d5ba-4868-8f08-a80d131c609d)
    
- `kubectl rollout history deployment <deployment-name>`

  - This command displays the revision history of a Deployment.
  - It shows a list of all revisions of the specified Deployment, including details about each revision. This helps track changes over time and understand what has been deployed previously.
  -  When to Use:
      1. Tracking Changes: To view the history of updates to a Deployment, including what changes were made and when.
      2. Audit and Troubleshoot: To review past deployments and understand the evolution of your application.
         
     ![image](https://github.com/user-attachments/assets/e5715338-b8b7-49c9-9ab6-aad71351ed34)
     
- `kubectl rollout undo deployment <deployment-name>` or `kubectl rollout undo deployment/<deployment-name> --to-revision=2`

  - It rolls back the Deployment to its previous revision, which is useful if the most recent update has caused issues or failed.
  - This command restores the Deployment to the state it was in before the latest change.
  - When to Use:
    1. Reverting Changes: If a recent deployment introduces issues or fails, use this command to roll back to a previous, stable version.
    2. Quick Fix: To quickly undo problematic updates and restore a previously working state of the application.
    
    ![image](https://github.com/user-attachments/assets/afa59084-01cc-41df-8c41-74fd015aace6)

    ##### Verifying

    ![image](https://github.com/user-attachments/assets/7125a5cb-3108-4ff3-b4dd-bfa794600958)

**NOTE : In Deployment, the number of replicas (Pods) remains constant during updates and rollbacks unless explicitly changed.**


## Reasons for a Deployment to Fail

1. **Configuration Issues**: Errors in the YAML/JSON manifest or incorrect container image names.

2. **Resource Constraints**: Insufficient CPU or memory, or exceeding resource quotas.

3. **Pod Scheduling Issues**: Problems with node affinity, taints/tolerations, or lack of available nodes.

4. **Image Pull Errors**: Issues with pulling container images due to incorrect names or authentication problems.

5. **Health Check Failures**: Readiness or liveness probes failing, or application startup issues.

6. **Networking Issues**: Problems with service discovery, network policies, or connectivity.

7. **Version Mismatches**: Incompatible updates or changes in Kubernetes APIs.

8. **Rollout Issues**: Problems during rolling updates or rollbacks.

9. **Permission Issues**: Insufficient permissions for service accounts or users.

10. **Deployment Strategy Misconfiguration**: Incorrect settings for update strategies.

**Troubleshooting Tips**:
- Check logs
- Describe resources
- View events
- Monitor resource usage
