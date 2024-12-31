+++
title = "Part 06"

+++

## Healthcheck Probes in Kubernetes

In Kubernetes, health checks are implemented using **liveness**, **readiness**, and **startup** probes. These probes ensure the application is running correctly and capable of serving requests. Below are examples for each type of probe.

## Types of Probes

1. **Liveness Probe**  
   Ensures the application is running. If the liveness probe fails, Kubernetes restarts the container.

2. **Readiness Probe**  
   Ensures the application is ready to serve traffic. If the readiness probe fails, Kubernetes removes the pod from the service endpoints.

3. **Startup Probe**  
   Ensures the application has started. It is useful for containers that take a long time to initialize.

## Example Configuration

Below is an example of a pod configuration with all three types of probes:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: healthcheck-example
spec:
  containers:
  - name: example-container
    image: nginx
    ports:
    - containerPort: 80
    livenessProbe:
      httpGet:
        path: /healthz
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 10
    readinessProbe:
      httpGet:
        path: /readiness
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 10
    startupProbe:
      httpGet:
        path: /startup
        port: 80
      initialDelaySeconds: 10
      periodSeconds: 10
```


## Kubernetes Pod Scheduling 
- [Follow this tutorial for Kubernetes pod scheduling](https://k8s.selfstudy.space/part02/index.html)
## Kubernetes Storage 
- [Follow Documentation for Kubernetes Storage](https://k8s.selfstudy.space/part04/index.html)
  - Block Storage 
  - Object Storage
  - File Storage
  - Secret
  - Configmap
  


