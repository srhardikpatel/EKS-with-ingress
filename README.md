# Step by Step: How to Deploy a Scalable Application on AWS EKS using Ingress Controller.

## Create an EC2 Instance from the AWS management console.

## Command to install AWS CLI on to the EC2 instance.

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install
```

## Setup AWS Credentials using AWS Configure

## Install EKSCTL

> https://docs.aws.amazon.com/eks/latest/eksctl/installation.html

## Install Kubectl

> https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html

## Create a EKS Cluster without nodegroup (replace tag value)

```
eksctl create cluster --name=<cluster-name> --region=<region> --managed=false --version=<EKS-Kubernetes-version> --without-nodegroup
```

## Create a node
#### node-type = EC2-instance-type

```
eksctl create nodegroup \
  --cluster <cluster-name> \
  --name <node-name> \
  --node-type <node-type> \
  --node-ami-family <AMI>
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 2 \
  --ssh-access \
  --managed=false \
  --ssh-public-key <aws_key_name>
```

### OR (Combibe both above command in one)

```
eksctl create cluster \
    --name <cluster-name> \
    --version <EKS-Kubernetes-version> \
    --region <region-name> \
    --nodegroup-name <node-name> \
    --node-type <node-type> \
    --node-ami-family Ubuntu2404
    --nodes 2 \
    --nodes-min 1 \
    --nodes-max 3 \
    --ssh-access \
    --ssh-public-key <aws_key_name> \
    --managed=false
```
## Install HELM

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4
chmod 700 get_helm.sh
./get_helm.sh
helm version
```
## Install nginx ingress controller

```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

helm upgrade --install ingress-nginx ingress-nginx --repo https://kubernetes.github.io/ingress-nginx --namespace ingress-nginx --create-namespace
```

## Without HELM

```kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.14.3/deploy/static/provider/cloud/deploy.yaml```

## Git Clone

> https://github.com/srhardikpatel/EKS-with-ingress.git

## Create a pod using the deplyment file

```
kubectl apply -f namespace.yml
kubectl apply -f todo.yml -f java-app
kubectl apply -f ingress.yml
```

## To get the loadbalancer URl

```
kubectl get ingress -n <Namespace-name>
```

### Access the application

```
http://<loadbalancer-url>/todos
http://<loadbalancer-url>/java-app
```

# :boom: :boom: Congratulations :boom: :boom:
