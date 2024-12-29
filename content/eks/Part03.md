+++
title = "Part 03"

+++

## Steps to Create EKS Cluster with Existing VPC and Custom VPC Using eksctl

### Prerequisites

Ensure you have the following prerequisites in place:

1. **AWS CLI** installed and configured.
2. **eksctl** installed on the VPS.
3. **IAM Permissions** for creating EKS clusters, VPCs, and related resources in AWS.
4. **kubectl** installed.

## Step 1: Install eksctl

If `eksctl` is not installed, follow these steps:

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/0.114.0/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```
- **Create a Custom VPC for EKS Cluster**
 ```bash
    eksctl create vpc \
    --name my-cluster-vpc \
    --cidr 10.0.0.0/16 \
    --subnets private=10.0.1.0/24,public=10.0.2.0/24 \
    --region us-west-2
 ```
 - **Create the EKS Cluster with Custom VPC**
 ```bash
  eksctl create cluster \
  --name my-cluster \
  --region us-west-2 \
  --vpc-private-subnets=10.0.1.0/24 \
  --vpc-public-subnets=10.0.2.0/24 \
  --vpc-id <your-vpc-id> \
  --nodegroup-name standard-workers \
  --node-type t2.micro \
  --nodes 3 \
  --nodes-min 1 \
  --nodes-max 4 \
  --managed
 ```
- `` Update kubeconfig``

```bash
 aws eks --region us-west-2 update-kubeconfig --name my-cluster
```
- **Add Additional Node Groups (Optional)**
```bash
 eksctl create nodegroup \
  --cluster my-cluster \
  --name additional-workers \
  --node-type t2.small \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 3 \
  --region us-west-2
```
- **Clean Up**
  ```bash
  eksctl delete cluster --name my-cluster --region us-west-2
  ```
     

