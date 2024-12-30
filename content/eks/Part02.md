+++
title = "Part 02"

+++
## ```eksctl``` 
### CLI for Amazon EKS
The official CLI for Amazon EKS.
`eksctl` is a command-line tool designed to simplify the management of Kubernetes clusters on Amazon Elastic Kubernetes Service (EKS). It provides an easy and efficient way to manage EKS clusters, worker nodes, and addons.

### Features
- Simplifies EKS cluster creation and deletion.
- Manages worker nodes and node groups.
- Supports YAML configuration for declarative cluster management.
- Enables cluster logging and addon upgrades.
- Provides multi-region support for managing multiple clusters.
The features that are currently implemented are:

   -  Create, get, list and delete clusters
   - Create, drain and delete nodegroups
   -  Scale a nodegroup
   -  Update a cluster
   -  Use custom AMIs
   -  Configure VPC Networking
   -  Configure access to API endpoints
   -  Support for GPU nodegroups
   -  Spot instances and mixed instances
   -  IAM Management and Add-on Policies
    - List cluster Cloudformation stacks
    - Install coredns
    - Write kubeconfig file for a cluster

### Installation

#### Linux
```bash
curl -LO "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz"
tar -xzf eksctl_Linux_amd64.tar.gz
sudo mv eksctl /usr/local/bin
```

- **Configure Aws cli**
```bash
aws configure
```
- **Create a basic cluster in minutes with just one command**
```bash
eksctl create cluster
```
A cluster will be created with default parameters:

   - exciting auto-generated name, e.g., fabulous-mushroom-1527688624
   - two m5.large worker nodes (this instance type - suits most common use-cases, and is good value for money)
  -  use the official AWS EKS AMI
  -  us-west-2 region
   - a dedicated VPC (check your quotas)

- **Check installed EKS cluster**
```bash
eksctl get cluster
```
- Clean up kubernetes Cluster
```bash
eksctl delete cluster <cluster-name>
```
- **Create a Kubernetes cluster using declarative method**
```bash
cat <<EOF >cluster.yaml
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: basic-cluster
  region: eu-north-1

nodeGroups:
  - name: ng-1
    instanceType: m5.large
    desiredCapacity: 10
  - name: ng-2
    instanceType: m5.xlarge
    desiredCapacity: 2
EOF
 ```
- **Run the below command**
 ```bash
  eksctl create cluster -f cluster.yaml
 ```
### Basic cluster creation

 - **To create a basic cluster, but with a different name, run:**
```bash
eksctl create cluster --name=cluster-1 --nodes=4
```
- **Cluster credentials**

- **To write cluster credentials to a file other than default, run:**
```bash
eksctl create cluster --name=cluster-2 --nodes=4 --kubeconfig=./kubeconfig.cluster-2.yaml
```
-  **To prevent storing cluster credentials locally, run:**
```bash
eksctl create cluster --name=cluster-3 --nodes=4 --write-kubeconfig=false
```
- **To let eksctl manage cluster credentials under ~/.kube/eksctl/clusters directory, run:**
```bash
eksctl create cluster --name=cluster-3 --nodes=4 --auto-kubeconfig
```
- **To obtain cluster credentials at any point in time, run:**
```bash
eksctl utils write-kubeconfig --cluster=<name> [--kubeconfig=<path>] [--set-kubeconfig-context=<bool>]eksctl utils write-kubeconfig --cluster=<name> [--kubeconfig=<path>] [--set-kubeconfig-context=<bool>]
```

- **Caching Credentials**

eksctl supports caching credentials. This is useful when using MFA and not wanting to continuously enter the MFA token on each eksctl command run.

- **To enable credential caching set the following environment property EKSCTL_ENABLE_CREDENTIAL_CACHE as such:**
```bash
export EKSCTL_ENABLE_CREDENTIAL_CACHE=1
```
By default, this will result in a cache file under ~/.eksctl/cache/credentials.yaml which will contain creds per profile that is being used. To clear the cache, delete this file.

### Autoscaling

- **To use a 3-5 node Auto Scaling Group, run:**
```bash
eksctl create cluster --name=cluster-5 --nodes-min=3 --nodes-max=5
```

#### SSH access

In order to allow SSH access to nodes, eksctl imports ~/.ssh/id_rsa.pub by default, to use a different SSH public key, e.g. my_eks_node_id.pub, run:
```bash
eksctl create cluster --ssh-access --ssh-public-key=my_eks_node_id.pub
```

- **To use a pre-existing EC2 key pair in us-east-1 region, you can specify key pair name (which must not resolve to a local file path), e.g. to use my_kubernetes_key run:**
```bash
eksctl create cluster --ssh-access --ssh-public-key=my_kubernetes_key --region=us-east-1
```
AWS Systems Manager (SSM) is enabled by default, so it can be used to SSH onto nodes.
```bash
eksctl create cluster --enable-ssm
```
If you are creating managed nodes with a custom launch template, the --enable-ssm flag is disallowed.

#### Tagging

- **To add custom tags for all resources, use --tags.**
```bash
eksctl create cluster --tags environment=staging --region=us-east-1
```
#### Volume size

- **To configure node root volume, use the --node-volume-size (and optionally --node-volume-type), e.g.:**
```bash
eksctl create cluster --node-volume-size=50 --node-volume-type=io1
```


The default volume size is 80G.

- Yaml fils are stored at below location:
 [Yaml Files](https://github.com/eksctl-io/eksctl/tree/main/examples)