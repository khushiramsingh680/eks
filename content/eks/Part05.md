+++
title = "Part 05"
+++

## Topics to do 
- [Pod](#what-is-a-pod) 
- [Key Characteristics](#key-characteristics)
- [Types of Pods](#types-of-pods)
- [Pod Lifecycle](#pod-lifecycle)
- [Pod Management](#pod-management)
- [Pod Networking](#pod-networking)
- [Pod Security](#pod-security)
- [Volumes in Pods](#volumes-in-pods)
- [Pod Health Checks](#pod-health-checks)
- [initContainer](#kubernetes-init-containers)
- [Services](#kubernetes-services)
     - [NodePort](#nodeport-service)
     - [LoadBalancer](#loadbalancer-service)
     - [ExternalName](#externalname-service)
     - [External IP](#external-ip-service)
     - [ClusterIP](#1-clusterip-service)
- [Port and Service forwarding](#port-and-service-forwarding)
## Kubernetes Pod Concepts

## What is a Pod?

A **Pod** is the smallest deployable unit in Kubernetes. It represents a single instance of a running process in a cluster. Pods can contain one or more containers (e.g., Docker containers) that share the same network namespace, storage volumes, and are tightly coupled.

### Key Characteristics:
- **Single IP address**: All containers in the pod share the same network IP, enabling them to communicate via localhost.
- **Shared Storage**: Pods can define storage volumes shared by all containers in the pod.
- **Co-location**: Containers in a pod are co-located and can share resources, making them suitable for applications that require tight coupling.

## Types of Pods

### 1. **Single-container Pods**
   - **Definition**: A pod that contains only one container.
   - **Use Case**: Typically used for simple applications where only one container is required.

### 2. **Multi-container Pods**
   - **Definition**: A pod that contains more than one container, often for containers that need to work closely together.
   - **Use Case**: Often used for helper or sidecar containers that enhance the primary container's functionality (e.g., logging, monitoring).

## Pod Lifecycle

A pod goes through a series of states during its lifecycle:

1. **Pending**: The pod has been accepted by the Kubernetes system, but one or more containers have not yet been created.
2. **Running**: The containers in the pod are running and healthy.
3. **Succeeded**: All containers in the pod have terminated successfully.
4. **Failed**: One or more containers in the pod have terminated with an error.
5. **Unknown**: The state of the pod could not be determined.

## Pod Management

### 1. **Pod Scheduling**
   - Kubernetes uses the **Scheduler** to assign pods to nodes based on resource availability, affinity/anti-affinity rules, taints, and tolerations.

### 2. **Pod Disruption Budgets (PDB)**
   - **Definition**: Specifies the minimum number of pods that must be available during voluntary disruptions (e.g., node maintenance).

### 3. **Pod Autoscaling**
   - **Horizontal Pod Autoscaler (HPA)**: Automatically scales the number of pod replicas based on CPU usage or custom metrics.
   - **Vertical Pod Autoscaler (VPA)**: Automatically adjusts the CPU and memory resources allocated to a pod.

## Pod Networking

- **Pod-to-Pod Communication**: Pods can communicate with each other directly via their IP addresses.
- **Services**: Pods are often exposed through Kubernetes Services to enable load balancing and stable networking.
  
## Common Pod Annotations and Labels

- **Labels**: Key-value pairs used to organize and select groups of pods.
- **Annotations**: Key-value pairs used to store non-identifying metadata for pods, which are not used for selection but for configuration.

## Pod Security

- **Security Context**: Defines privilege and access control settings for containers in a pod.
- **Pod Security Policies**: Deprecated, but formerly used to define constraints on pod specifications to enhance security.

## Volumes in Pods

Pods can use **Volumes** to share data between containers, and volumes persist across container restarts. Examples of volumes:
- **EmptyDir**: A temporary storage for the lifetime of the pod.
- **PersistentVolumes**: External storage resources that pods can mount for data persistence.
- **ConfigMap and Secret Volumes**: Mounts data from Kubernetes ConfigMaps and Secrets as files inside the pod.

## Pod Health Checks

- **Liveness Probe**: Checks whether the pod's containers are running.
- **Readiness Probe**: Checks whether the pod's containers are ready to serve traffic.
- **Startup Probe**: Determines if the pod's containers have started successfully.

## Pod Resource Management

- **Requests**: The minimum amount of CPU and memory guaranteed for the pod.
- **Limits**: The maximum amount of CPU and memory a pod can use.

## Pod Disruption and Eviction

- **Pod Disruption**: Voluntary disruptions (e.g., node draining) that can affect pod availability.
- **Eviction**: Occurs when a node runs out of resources, leading to pods being evicted to free up resources for more critical workloads.

---

For detailed usage, configurations, and command examples related to Pods, refer to Kubernetes documentation or specific deployment needs.
## Kubernetes Init Containers

An **Init Container** is a special type of container that runs before the main containers in a Pod are started. They are typically used for initialization tasks, such as setting up configurations, checking prerequisites, or preparing the environment before the main application starts.

## 1. Why Use Init Containers?
- They can perform tasks that must happen before the main containers.
- They help separate concerns (e.g., downloading a configuration file from a remote source).
- They can be used to manage dependencies, e.g., waiting for a service to be available.

## 2. Characteristics of Init Containers:
- They run to completion before the main containers start.
- If an Init Container fails, Kubernetes will retry it according to its restart policy.
- Init Containers are defined in the Pod specification, just like regular containers, but under the `initContainers` field.
- They run sequentially—each Init Container must complete before the next one begins.

## 3. Example YAML with Init Containers:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-initcontainer
spec:
  initContainers:
    - name: init-myservice
      image: busybox
      command: ['sh', '-c', 'echo "Initializing the environment" && sleep 5']
      resources:
        limits:
          memory: "64Mi"
          cpu: "250m"
      volumeMounts:
        - name: shared-data
          mountPath: /data
  containers:
    - name: main-app
      image: nginx
      ports:
        - containerPort: 80
      volumeMounts:
        - name: shared-data
          mountPath: /usr/share/nginx/html
  volumes:
    - name: shared-data
      emptyDir: {}
```
## Pod Related Commands

### 1. Create a Pod
```bash
kubectl run mypod --image=nginx
```
### 2. Create a Pod from YAML
```bash
kubectl apply -f pod.yaml
```
### 3. View Pod Details
```bash
kubectl describe pod mypod
```
### 4. List All Pods
```bash
kubectl get pods
```
### 5. List Pods in All Namespaces
```bash
kubectl get pods --all-namespaces
```
### 6. Get Pod Logs
```bash
kubectl logs mypod
```
### 7. View Logs of a Specific Container
```bash
kubectl logs mypod -c mycontainer
```
### 8. View Previous Pod Logs
```bash
kubectl logs mypod --previous
```
### 9. Get Pod Logs with Timestamps
```bash
kubectl logs mypod --timestamps
```
### 10. Stream Pod Logs
```bash
kubectl logs -f mypod
```
### 11. Exec into a Pod
```bash
kubectl exec -it mypod -- /bin/bash
```
### 12. Run a Command in a Specific Container
```bash
kubectl exec -it mypod -c mycontainer -- /bin/bash
```
### 13. Check Pod Resource Usage
```bash
kubectl top pod mypod
```
### 14. Scale a Deployment (Pods)
```bash
kubectl scale deployment mydeployment --replicas=3
```
### 15. Delete a Pod
```bash
kubectl delete pod mypod
```
### 16. Delete All Pods in a Namespace
```bash
kubectl delete pods --all --namespace=mynamespace
```
### 17. Delete Pods by Label
```bash
kubectl delete pods -l app=myapp
```
### 18. Get Pod Events
```bash
kubectl get events --field-selector involvedObject.name=mypod
```
### 19. Apply a Pod Configuration
```bash
kubectl apply -f pod.yaml
```
### 20. Show Pod Details in Wide Format
```bash
kubectl get pods -o wide
```
### 21. Check Pod Status
```bash
kubectl get pod mypod -o=jsonpath='{.status.phase}'
```
### 22. Get Pod Resource Requests and Limits
```bash
kubectl get pod mypod -o=jsonpath='{.spec.containers[*].resources}'
```
### 23. Create a Pod with Multiple Containers
```bash
kubectl apply -f multi-container-pod.yaml
```
### 24. Check Pod’s Running Nodes
```bash
kubectl get pod mypod -o=jsonpath='{.spec.nodeName}'
```
### 25. Create Pod with Host Network
```bash
kubectl run mypod --image=nginx --host-network=true
```
### 26. Attach to a Pod’s Container
```bash
kubectl attach -it mypod -c mycontainer
```
### 27. Port Forward to a Pod
```bash
kubectl port-forward pod/mypod 8080:80
```
### 28. Check Pod’s IP Address
```bash
kubectl get pod mypod -o=jsonpath='{.status.podIP}'
```
### 29. Get Pod in JSON Format
```bash
kubectl get pod mypod -o json
```
### 30. Get Pod in YAML Format
```bash
kubectl get pod mypod -o yaml
```
### 31. Set Pod Annotations
```bash
kubectl annotate pod mypod key=value
```
### 32. Set Pod Labels
```bash
kubectl label pod mypod key=value
```
### 33. Check Pod's Node Affinity
```bash
kubectl get pod mypod -o=jsonpath='{.spec.affinity.nodeAffinity}'
```
### 34. Create a Pod with a Persistent Volume
```bash
kubectl apply -f pod-with-pv.yaml
```
### 35. Evict a Pod
```bash
kubectl evict mypod
```
### 36. Create Pod with Resource Requests and Limits
```bash
kubectl apply -f pod-with-resources.yaml
```
### 37. Check Pod Health (Probes)
```bash
kubectl describe pod mypod
```
### 38. Create a Pod with Secrets
```bash
kubectl apply -f pod-with-secret.yaml
```
### 39. Create a Pod with ConfigMap
```bash
kubectl apply -f pod-with-configmap.yaml
```
### 40. Check Pod’s Resource Usage in All Namespaces
```bash
kubectl top pod --all-namespaces
```
### 41. Create a Pod with Init Containers
```bash
kubectl apply -f pod-with-init-containers.yaml
```
### 42. Check Pod Events in JSON
```bash
kubectl get events --field-selector involvedObject.name=mypod -o json
```
### 43. View the Pod’s Resource Quotas
```bash
kubectl describe resourcequota
```
### 44. View the Pod’s Service Account
```bash
kubectl get pod mypod -o=jsonpath='{.spec.serviceAccountName}'
```
### 45. Run a Pod in the Background
```bash
kubectl run mypod --image=nginx --restart=Never
```
### 46. Get All Pods in a Specific Namespace
```bash
kubectl get pods -n mynamespace
```
### 47. Check Pod’s Restart Count
```bash
kubectl get pod mypod --show-labels
```
### 48. Edit a Pod’s Configuration
```bash
kubectl edit pod mypod
```
### 49. Set Pod Environment Variables
```bash
kubectl set env pod/mypod VAR_NAME=value
```
### 50. Delete a Pod Using Label Selector
```bash
kubectl delete pod -l app=myapp
```

## LAB 01
-  Login to a pod
-  Login to a specific container 
-  check logs for a container
-  Copy files from local to pod and vice versa
-  Check the usages of pod
-  Check the ip of pods
-  check where the pod is running 
-  Describe the pods
-  Check the pod is in pending state 
-  Check kubernetes events 
-  Delete a pod 
-  Delete a pod forcefully
-  Check the pod name from a namespace 
-  For loop to check the name of all pod and delete them 
-  Check the pod with labels

## Kubernetes Services

## Overview
In Kubernetes, a **Service** is an abstraction that defines a logical set of Pods and a policy by which to access them. Services provide a stable IP address and DNS name for a set of Pods, ensuring seamless communication between components, even if Pods are replaced or restarted. 

### Key Features of Services:
- Decouples applications from underlying Pods.
- Provides a single endpoint for accessing multiple Pods.
- Ensures high availability by automatically routing traffic to healthy Pods.

---

## Types of Kubernetes Services

Kubernetes offers several service types to cater to different use cases:

1. **ClusterIP** (default)
2. **NodePort**
3. **LoadBalancer**
4. **ExternalName**
5. **External IP**

Each service type has specific use cases, benefits, and limitations. Below are the details.

---

## 1. ClusterIP Service

A **ClusterIP** service is the default service type in Kubernetes. It exposes the service only within the cluster, making it accessible from other services and Pods.

### Use Cases
- Internal communication between services.
- Backend services that are not exposed to external traffic.

### YAML Example
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-clusterip-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP
```
- **List all ClusterIP services:**
```bash
kubectl get svc --field-selector spec.type=ClusterIP
```

## NodePort Service

A NodePort service exposes the application on a static port (NodePort) on each node's IP address. External users can access the application using <NodeIP>:<NodePort>.
Use Cases

  -  Exposing applications for external access.
  -  Quick testing or debugging environments.
```bash
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30001
  type: NodePort
```
 ## LoadBalancer Service

A LoadBalancer service exposes the application to the internet by provisioning an external load balancer. This service type is supported by most cloud providers.
Use Cases

   -  Hosting internet-facing applications.
   - Automatically balancing traffic across multiple Pods.
```bash
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```
## ExternalName Service

An ExternalName service maps a service to an external DNS name. It does not create a proxy or load balancer but allows Kubernetes to act as a DNS resolver for external services.
Use Cases

   - Accessing external resources like databases or APIs from within the cluster.
```bash
apiVersion: v1
kind: Service
metadata:
  name: my-external-service
spec:
  type: ExternalName
  externalName: example.com
```
## External IP Service

An External IP service allows a specific external IP address to be routed to one or more Pods in the cluster.
Use Cases

  -  Exposing services to external networks using a predefined IP.
  -  Interfacing with on-premises applications or devices.
```bash
apiVersion: v1
kind: Service
metadata:
  name: my-externalip-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  externalIPs:
    - 192.168.1.100
```


## Port and Service Forwarding

## Overview
Port forwarding allows you to access a service or Pod locally without exposing it to external traffic. This is useful for debugging or testing applications running inside the cluster.

## Commands

### Port Forward a Pod to a Local Port
```bash
kubectl port-forward pod/<pod-name> 8080:80
```

### Port Forward a Service to a Local Port
```bash
kubectl port-forward svc/<service-name> 9090:80
```

## Kubernetes 
 - Resource Quota
 - Resource Limits
 - QOS
     - Burstable
     - Guarenteed
  