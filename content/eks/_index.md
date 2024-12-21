+++
title = "Elastic Kubernetes Service (EKS)"
type = "chapter"
weight = 1
+++

# AWS EKS and Kubernetes Learning Guide

## Cluster Setup and Configuration

### Create AWS EKS Cluster using eksctl CLI
- Install and configure `eksctl`.
- Create an EKS cluster with worker nodes.
- Set up `kubectl` to manage the cluster.
- Verify the cluster resources and configuration.

---

## Fundamentals

### Docker Fundamentals
- Understand containers vs. virtual machines.
- Learn essential Docker commands for container lifecycle management.
- Build, tag, and push Docker images.
- Work with Docker registries, such as AWS ECR.

### Kubernetes Fundamentals
- **Pods**: Core Kubernetes objects and their lifecycle.
- **ReplicaSets**: Manage pod replication and high availability.
- **Deployments**: Rollouts, updates, and rollbacks.
- **Services**:  
  - NodePort Service.  
  - ClusterIP Service.  
  - LoadBalancer Service.  
  - ExternalName Service.  

---

## EKS Storage Solutions

### EKS Storage with AWS EBS CSI Driver
- Install and configure the AWS EBS CSI Driver.
- Create Persistent Volumes (PV) and Persistent Volume Claims (PVC).
- Mount EBS volumes to Kubernetes pods.

### EKS Storage with AWS RDS MySQL Database
- Integrate an RDS MySQL database with Kubernetes workloads.
- Use Kubernetes Secrets to store database credentials securely.

---

## Kubernetes Important Concepts for Application Deployments

### Kubernetes - Secrets
- Securely manage sensitive data like passwords and API keys.
- Create, update, and mount secrets in pods.

### Kubernetes - Init Containers
- Configure Init Containers to run setup tasks before the main container starts.

### Kubernetes - Liveness & Readiness Probes
- Implement health checks for application containers.
- Configure probes to restart unhealthy containers or delay traffic to unready pods.

### Kubernetes - Requests & Limits
- Manage resource requests and limits to optimize workload performance.

### Kubernetes - Namespaces, LimitRange, and Resource Quota
- Use namespaces to organize resources.
- Set up LimitRange and ResourceQuota for better resource control.

---

## Load Balancing Solutions

### Load Balancing using CLB - AWS Classic Load Balancer
- Expose Kubernetes services with AWS Classic Load Balancer.

### Load Balancing using NLB - AWS Network Load Balancer
- Configure AWS NLB for high-performance load balancing.

### Load Balancing using ALB - AWS Application Load Balancer
- **ALB Ingress Controller - Install**: Set up ALB Ingress Controller for Kubernetes.
- **ALB Ingress - Basics**: Basic configurations for ALB ingress.
- **ALB Ingress - Context Path-based Routing**: Route requests based on paths.
- **ALB Ingress - SSL**: Enable SSL with AWS Certificate Manager.
- **ALB Ingress - SSL Redirect (HTTP to HTTPS)**: Enforce HTTPS traffic.
- **ALB Ingress - External DNS**: Integrate with Route53 for DNS management.

---

## Serverless Deployments

### Deploy Kubernetes Workloads on AWS Fargate
- Deploy applications on AWS Fargate for serverless computing.
- Configure **Fargate Profiles**:
  - Basic profiles.
  - Advanced profiles using YAML.

---

## CI/CD and Monitoring

### Build and Push Containers to AWS ECR
- Build Docker images and push them to AWS ECR.
- Deploy ECR-based images to EKS.

### DevOps with AWS Developer Tools
- Use CodeCommit, CodeBuild, and CodePipeline for CI/CD pipelines.

### Microservices Deployment on EKS
- **Service Discovery**: Enable service discovery in Kubernetes.
- **Distributed Tracing with AWS X-Ray**: Track requests in microservices.
- **Canary Deployments**: Gradually roll out updates to reduce risk.

### EKS Monitoring using CloudWatch
- Set up CloudWatch Agent and Fluentd for Container Insights.
- Monitor logs, metrics, and alarms using CloudWatch.

---

## Kubernetes Scaling

### EKS HPA - Horizontal Pod Autoscaler
- Configure HPA to scale pods based on resource metrics.

### EKS VPA - Vertical Pod Autoscaler
- Automatically adjust pod resource requests and limits.

### EKS CA - Cluster Autoscaler
- Scale worker nodes based on demand.

---

## AWS Services Covered

- **AWS EKS**: Elastic Kubernetes Service.
- **AWS EBS**: Elastic Block Store.
- **AWS RDS**: Relational Database Service (MySQL).
- **AWS CLB**: Classic Load Balancer.
- **AWS NLB**: Network Load Balancer.
- **AWS ALB**: Application Load Balancer.
- **AWS Fargate**: Serverless computing for Kubernetes.
- **AWS ECR**: Elastic Container Registry.
- **AWS Developer Tools**: CodeCommit, CodeBuild, CodePipeline.
- **AWS X-Ray**: Distributed tracing for microservices.
- **AWS CloudWatch**: Container Insights, Log Groups, Log Insights, and Alarms.
- **AWS Route53**: DNS management.
- **AWS Certificate Manager**: SSL/TLS certificates.
- **AWS SNS**: Simple Notification Service.

---

## Kubernetes Concepts Covered

### Kubernetes Architecture
- Understand the core components of Kubernetes.

### Core Objects
- **Pods**: Manage containerized applications.
- **ReplicaSets**: Ensure pod availability.
- **Deployments**: Manage application updates.
- **Services**:  
  - NodePort, ClusterIP, ExternalName, and LoadBalancer services.

### Kubernetes Networking
- **Ingress Services**:  
  - SSL configuration and redirection.  
  - Integration with external DNS.

### Resource Management
- **Requests & Limits**: Optimize workload performance.
- **Namespaces**:  
  - Create namespaces imperatively.  
  - Configure LimitRange and ResourceQuota.  

### Storage
- **Storage Classes**: Automate PV provisioning.
- **Persistent Volumes (PV)**: Manage storage resources.
- **Persistent Volume Claims (PVC)**: Request storage dynamically.

### Advanced Concepts
- **Secrets**: Manage sensitive information securely.
- **Init Containers**: Pre-start initialization tasks.
- **Liveness & Readiness Probes**: Ensure application health.
- **Annotations**: Add metadata to Kubernetes objects.

### Scaling and Autoscaling
- **HPA - Horizontal Pod Autoscaler**: Scale pods based on metrics.
- **VPA - Vertical Pod Autoscaler**: Adjust resource allocation dynamically.
- **CA - Cluster Autoscaler**: Scale nodes automatically.

### Logging and Monitoring
- **DaemonSets**: Fluentd DaemonSet for centralized logging.
- **ConfigMaps**: Manage application configuration data.

### Deployment Strategies
- **Canary Deployments**: Gradual updates for testing and stability.
