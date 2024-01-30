# To Setup Kubernetes Cluster on AWS Cloud using kops (kubernetes operations)

## What is KOPS?

Kops (Kubernetes Operations) used to you create, destroy, upgrade and maintain production-grade, highly available, Kubernetes clusters using command line.

## Prerequisites

1. Create a hosted zone in AWS Route53
2. Create a s3 bucket with versioning
3. IAM user with Access
4. Ubuntu based EC2 instance
5. AWS CLI configure

### Create a Hosted zone in Route53

![image](https://github.com/kohlidevops/kops-to-setup-kubernetes/assets/100069489/e9200272-b765-4dc6-850c-66e6d4cdcaf1)

### Create a S3 bucket with versioning

To create a S3 bucket with versioning to store kubernetes kops cluster state

![image](https://github.com/kohlidevops/kops-to-setup-kubernetes/assets/100069489/81cf3cca-7740-41cc-b2a7-4319bef61dfe)

### IAM User with permissions

![image](https://github.com/kohlidevops/kops-to-setup-kubernetes/assets/100069489/3772d1c6-942f-43ce-96ff-374707afd137)

Create accesskey and secretkey for IAM user to use later

### Launch ubuntu22 EC2 instance

To launch ubunut22 EC2 instance to setup kubernetes cluster using kops

![image](https://github.com/kohlidevops/kops-to-setup-kubernetes/assets/100069489/0515bb38-1bd5-4945-a6f2-fc93d56262f0)

SSH to machine and perform below commands

```
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install zip unzip -y
sudo apt-get install net-tools -y
```

To download the latest AWS CLI version for 64 bit Linux using curl use below command

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

![image](https://github.com/kohlidevops/kops-to-setup-kubernetes/assets/100069489/9d247dfd-ba51-4db2-b253-75b0114f46e4)

### Install Kubectl Binary with CURL on Ubuntu

```
sudo curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
sudo chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

### Install KOPS on Ubuntu Instance

To download the KOPS  setup on Ubuntu using curl

```
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
sudo chmod +x kops-linux-amd64
sudo mv kops-linux-amd64 /usr/local/bin/kops
```

### Create and configure IAM User in AWS

I have created IAM user with required permissions and created accesskey / secretkey.

```
aws configure
```

### To export kops state

I already created S3 bucket with versioning enabled.

```
export KOPS_STATE_STORE=s3://k8.kkawslearning.net
```

### Create SSH keys

To create ssh keys on Ubuntu instance to exchange kubernetes cluster and connect

```
ssh-keygen
```

### Setup Kubernetes on AWS using KOPS

To create Kubernetes  on AWS using Kops using below command

```
kops create cluster --cloud=aws --zones=ap-south-1a --networking calico --name=k8.kkawslearning.net --dns-zone=k8.kkawslearning.net --dns public
```
![image](https://github.com/kohlidevops/kops-to-setup-kubernetes/assets/100069489/19f3c50d-0398-42b8-b169-884b2d5202e6)

To configure the k8s kops cluster using below command

```
kops update cluster --name k8.kkawslearning.net --yes
```

![image](https://github.com/kohlidevops/kops-to-setup-kubernetes/assets/100069489/9461c7e7-1e5c-404e-bb33-deaf5d7f65b0)

To export the kubernetes configuration files with administrative privileges

```
kops export kubecfg --admin
```

To validate the cluster - Wait for sometime

```
kops validate cluster
```

![image](https://github.com/kohlidevops/kops-to-setup-kubernetes/assets/100069489/bd30a85a-6846-4a42-bcc9-db06dec733bf)

![image](https://github.com/kohlidevops/kops-to-setup-kubernetes/assets/100069489/cac827c0-660c-46e8-9853-f07ab10bd26e)

To check cluster info

```
kubectl cluster-info
```

