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
     
## EKS - Create EKS Node Group in Private Subnets

## Step-01: Introduction
- We are going to create a node group in VPC Private Subnets
- We are going to deploy workloads on the private node group wherein workloads will be running private 
subnets and load balancer gets created in public subnet and accessible via internet.

## Step-02: Delete existing Public Node Group in EKS Cluster
```
# Get NodeGroups in a EKS Cluster
eksctl get nodegroup --cluster=<Cluster-Name>
eksctl get nodegroup --cluster=eksdemo1

# Delete Node Group - Replace nodegroup name and cluster name
eksctl delete nodegroup <NodeGroup-Name> --cluster <Cluster-Name>
eksctl delete nodegroup eksdemo1-ng-public1 --cluster eksdemo1
```

## Step-03: Create EKS Node Group in Private Subnets
- Create Private Node Group in a Cluster
- Key option for the command is `--node-private-networking`

```
eksctl create nodegroup --cluster=eksdemo1 \
                        --region=us-east-1 \
                        --name=eksdemo1-ng-private1 \
                        --node-type=t3.medium \
                        --nodes-min=2 \
                        --nodes-max=4 \
                        --node-volume-size=20 \
                        --ssh-access \
                        --ssh-public-key=kube-demo \
                        --managed \
                        --asg-access \
                        --external-dns-access \
                        --full-ecr-access \
                        --appmesh-access \
                        --alb-ingress-access \
                        --node-private-networking                       
```

## Step-04: Verify if Node Group created in Private Subnets

### Verify External IP Address for Worker Nodes
- External IP Address should be none if our Worker Nodes created in Private Subnets
```
kubectl get nodes -o wide
```
### Subnet Route Table Verification - Outbound Traffic goes via NAT Gateway
- Verify the node group subnet routes to ensure it created in private subnets
  - Go to Services -> EKS -> eksdemo -> eksdemo1-ng1-private
  - Click on Associated subnet in **Details** tab
  - Click on **Route Table** Tab.
  - We should see that internet route via NAT Gateway (0.0.0.0/0 -> nat-xxxxxxxx)



 ## AWS - Network Load Balancer - NLB

## Step-01: Create AWS Network Load Balancer Kubernetes Manifest & Deploy
- **04-NetworkLoadBalancer.yml**
```yml
apiVersion: v1
kind: Service
metadata:
  name: nlb-usermgmt-restapp
  labels:
    app: usermgmt-restapp
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-type: nlb    # To create Network Load Balancer
spec:
  type: LoadBalancer # Regular k8s Service manifest with type as LoadBalancer
  selector:
    app: usermgmt-restapp     
  ports:
  - port: 80
    targetPort: 8095
```
- **Deploy all Manifest**
```
## Deploy all manifests
kubectl apply -f kube-manifests/

## List Services (Verify newly created NLB Service)
kubectl get svc

## Verify Pods
kubectl get pods
```

## Step-02: Verify the deployment
- Verify if new CLB got created 
  - Go to  Services -> EC2 -> Load Balancing -> Load Balancers 
    - CLB should be created
    - Copy DNS Name (Example: a85ae6e4030aa4513bd200f08f1eb9cc-7f13b3acc1bcaaa2.elb.us-east-1.amazonaws.
com)
  - Go to  Services -> EC2 -> Load Balancing -> Target Groups
    - Verify the health status, we should see active. 
- **Access Application** 
```
# Access Application
http://<NLB-DNS-NAME>/usermgmt/health-status
```    

## Step-03: Clean Up 
```
## Delete all Objects created
kubectl delete -f kube-manifests/

## Verify current Kubernetes Objects
kubectl get all
```






