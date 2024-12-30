+++
title = "Part 12"

+++


## Node Management in Kubernetes

## Overview
Node management in Kubernetes involves maintaining and managing the nodes in your cluster. Nodes are the physical or virtual machines where Pods run. Efficient management is critical to ensuring the availability, scaling, and health of your cluster.

## Node Types

- **Master Node**: Manages the Kubernetes cluster and runs the control plane components (API server, scheduler, controller-manager, etc.).
- **Worker Node**: Hosts the Pods that run your applications.

## Commands for Node Management

### View Node Information
```bash
kubectl get nodes
```
## Node Management in Kubernetes

## Overview
Node management in Kubernetes involves maintaining and managing the nodes in your cluster. Nodes are the physical or virtual machines where Pods run. Efficient management is critical to ensuring the availability, scaling, and health of your cluster.

## Node Types
- **Master Node**: Manages the Kubernetes cluster and runs the control plane components (API server, scheduler, controller-manager, etc.).
- **Worker Node**: Hosts the Pods that run your applications.

## Commands for Node Management
### View Node Information
`kubectl get nodes`
- Lists all nodes in the cluster along with their status, roles, and other information.

### Describe a Specific Node
`kubectl describe node <node-name>`
- Provides detailed information about a specific node, including its capacity, conditions, and the list of Pods running on it.

### Cordon a Node
`kubectl cordon <node-name>`
- Marks the node as unschedulable, preventing new Pods from being scheduled onto it, but it allows existing Pods to continue running.

### Uncordon a Node
`kubectl uncordon <node-name>`
- Makes the node schedulable again, allowing new Pods to be scheduled onto it.

### Drain a Node
`kubectl drain <node-name> --ignore-daemonsets`
- Safely evicts all Pods from a node (except DaemonSets), preparing the node for maintenance or decommissioning.

### Delete a Node
`kubectl delete node <node-name>`
- Removes a node from the cluster.

## Node Affinity and Taints/Tolerations
### Setting Node Affinity
Node affinity is used to constrain which nodes your Pods can be scheduled on, based on labels on nodes. Example:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: "disktype"
                operator: In
                values:
                  - ssd
```



