AKS - Azure Kubernetes service
AKS is highly available, secure and fully managed k8s service.

## AKS Architecture
![](Pasted%20image%2020220702215227.png)


# ACR with AKS
## ACR
- Azure Container Registry is a managed, private Docker registry service 
- We can create and maintain Azure container registries to store and manage our private Docker container images. 
- With ACR, we can simplify our container lifecycle management. 
- Geo-replication to efficiently manage a single registry across multiple regions 
- Automated container building and patching including base image updates and task scheduling 
- Integrated security with Azure Active Directory (Azure AD) authentication, role-based access control, Docker Content Trust and virtual network integration.

## ACR Pricing tiers
![](Pasted%20image%2020220721003240.png)

## Azure ACR and AKS - Integration
![](Pasted%20image%2020220721002632.png)

```
-   Build a Docker Image from our Local Docker on our Desktop
-   Tag the docker image in the required ACR Format
-   Push to Azure Container Registry
-   Attach ACR with AKS
-   Deploy kubernetes workloads and see if the docker image got pulled automatically from ACR we have created.
```

### Create Azure Container Registry
-   Go to Services -> Container Registries
-   Click on **Add**
-   Subscription: StackSimplify-Paid-Subsciption
-   Resource Group: aks-rg2
-   Registry Name: acrforaksdemo2 (NAME should be unique across Azure Cloud)
-   Location: Central US
-   SKU: Basic (Pricing Note: $0.167 per day)
-   Click on **Review + Create**
-   Click on **Create**

### Build Docker Image Locally
Dockerfile
```yml
FROM nginx

COPY index.html /usr/share/nginx/html
```
Index.html
```
<!DOCTYPE html>

<html>

<body style="background-color:rgb(127, 178, 210);">

<h1>Welcome to Stack Simplify - Azure Container Registry (ACR)</h1>

<h2>Build and Push Image from Local Docker Desktop to ACR</h2>

<p>Application Version: V1</p>

</body>

</html>
```

```sh
# Change Directory
cd docker-manifests
 
# Docker Build
docker build -t kube-nginx-acr:v1 .

# List Docker Images
docker images
docker images kube-nginx-acr:v1

```

### Run Docker Container locally and test

```
# Run locally and Test
docker run --name kube-nginx-acr --rm -p 80:80 -d kube-nginx-acr:v1

# Access Application locally
http://localhost

# Stop Docker Image
docker stop kube-nginx-acr
```

### ## Enable Docker Login for ACR Repository

-   Go to Services -> Container Registries -> acrforaksdemo2
-   Go to **Access Keys**
-   Click on **Enable Admin User**
-   Make a note of Username and password

...
Ref: https://github.com/bhaaskara/azure-aks-kubernetes-masterclass/tree/master/18-Azure-Container-Registry-ACR/18-01-ACR-attach-to-AKS

## ACR and AKS - Service Principle - Run on AKS Nodepools
![](Pasted%20image%2020220721002721.png)
Ref: https://github.com/bhaaskara/azure-aks-kubernetes-masterclass/tree/master/18-Azure-Container-Registry-ACR/18-02-ACR-not-attached-to-AKS-Schedule-to-NodePools



## ACR and AKS - Service Principle - Run on Azure Virtual Nodes
![](Pasted%20image%2020220721002924.png)



# AKS Integrated with Azure AD
![](Pasted%20image%2020220805132813.png)

![](Pasted%20image%2020220723161207.png)

https://github.com/bhaaskara/azure-aks-kubernetes-masterclass/tree/master/21-Azure-AKS-Authentication-and-RBAC

1. Create Azure AD Users and Associate them with AD Group
2. Enable AKS Cluster with AKS-managed Azure Active Directory feature
3. Access an Azure AD enabled AKS cluster using Azure AD User
    `az aks get-credentials --resource-group aks-rg3 --name aksdemo3 --overwrite-existing`
  >  It does browser authentication first time.
  
## K8s Roles and Role bindings with Azure AD

# AKS Autoscaling
https://github.com/bhaaskara/azure-aks-kubernetes-masterclass/tree/master/22-Azure-AKS-Autoscaling

# AKS Virtual nodes (server less)
![](Pasted%20image%2020220724192911.png)

## Virtual Kubelet
- Virtual Kubelet is an open source Kubernetes kubelet implementation that acts as a kubelet for the purposes of connecting Kubernetes to other APIs. 
- This allows the k8s worker nodes to be backed by other services like Azure ACI and AWS Fargate 
- The primary scenario for VK is enabling the extension  of the Kubernetes API into serverless container platforms like Azure ACI and AWS Fargate.

![](Pasted%20image%2020220724195944.png)

**Current Features** 
- Create, delete and update pods 
- Container logs, exec, and metrics 
- get pod, pods and pod status 
- capacity 
- node addresses, node capacity, node daemon endpoints 
- operating system 
- bring your own virtual network 

## What is ACI (Azure Container Instance)
• Azure Container Instances (ACI) provide a hosted environment for running containers in Azure. 
•When using ACI, there is no need to manage the underlying compute infrastructure, Azure handles 
this management for you. 
• When running containers in ACI, you are charged per second for each running container

## Kubernetes Virtual Kubelet with Azure ACI
• The Azure Container Instances (ACI) provider for the Virtual Kubelet configures an ACI instance as a node 
in any Kubernetes cluster. 
• When using the Virtual Kubelet ACI provider, pods can be scheduled on an ACI instance as if the 
ACI instance is a standard Kubernetes node. 
•This configuration allows you to take advantage of both the capabilities of Kubernetes and the management value and cost benefit of ACI. 

**Virtual Kubelet's ACI provider - Features** 
• Volumes: empty dir, github repo, Azure Files 
• Secure env variables, config maps 
• Bring your own virtual network (VNet) 
• Network security group support 
• Basic Azure Networking support within AKS virtual node 
• Exec support for container instances 
• Azure Monitor integration or formally known as OMS 

## Azure AKS Virtual nodes (Server Less)
• We can run our Kubernetes workloads on Serverless Infrastructure of 
Azure which is called Virtual Nodes 
• Advantages 
    • Scale our applications rapidly without any limitations 
    • Quick Provisioning of pods using Virtual Nodes when compared to cluster 
autoscaler. 
    • Cluster Autoscaler need to provision the Nodes in a managed node pool first 
then only Kubernetes can schedule pods on those newly provisioned nodes. 
• Limitations (Huge and Many) 
https://docs.microsoft.com/en-us/azure/aks/virtual-nodes-cli#known- 
limitations 

![](Pasted%20image%2020220724201744.png)

![](Pasted%20image%2020220724201828.png)


# Production grade AKS cluster with Azure CLI
https://github.com/bhaaskara/azure-aks-kubernetes-masterclass/tree/master/23-AKS-Production-Grade-Cluster-Design-using-az-aks-cli

![](Pasted%20image%2020220724123424.png)

- ACR is connected through service principle or directly attaching to the AKS
- Azure monitor will be enabled by default if the cluster is created through portal.
   Azure monitor plugin should be enabled manually if cluster is created through CLI.
- Load balancer is very important to have enough IPs or bandwidth for outbound traffic.
- System assigned managed identity is super set of all other identity systems and is more recommended.

![](Pasted%20image%2020220724152637.png)

1. Create AKS Cluster using CLI
2. Create Node pools
3. Deplot applications on to nodepools using node selector

## Create AKS cluster using CLI
https://github.com/bhaaskara/azure-aks-kubernetes-masterclass/tree/master/23-AKS-Production-Grade-Cluster-Design-using-az-aks-cli/23-01-Create-AKSCluster-with-az-aks-cli

![](Pasted%20image%2020220724153124.png)

### Pre-requisite Items for AKS Cluster Design
-   Create Resource Group
-   Create Azure Virtual Network
    -   Create default Subnet for AKS Nodepools
    -   Create separate Subnet for Virtual Nodes (Serverless)
-   Create Azure AD User, Group for managing AKS Clusters using Azure AD Users
-   Create SSH Keys to enable and access Kubernetes Workernodes via SSH Terminal
-   Create Log Analytics Workspace for enabling Monitoring Add On during AKS Cluster creation
-   Set Windows Username and Password during AKS Cluster creation to have AKS Cluster support future Windows Nodepools

#### Pre-requisite-1: Create Resource Group
```
# Edit export statements to make any changes required as per your environment
# Execute below export statements
AKS_RESOURCE_GROUP=aks-prod
AKS_REGION=centralus
echo $AKS_RESOURCE_GROUP, $AKS_REGION

# Create Resource Group
az group create --location ${AKS_REGION} \
                --name ${AKS_RESOURCE_GROUP}
```

#### Pre-requisite-2: Create Azure Virtual Network and Two Subnets
-   Create Azure Virtual Network
-   Create Two subnets one for regular AKS Cluster and second one for Azure Virtual Nodes
    -   Subnet-1: aks-prod-default
    -   Subnet-2: aks-prod-virtual-nodes

```
# Edit export statements to make any changes required as per your environment
# Execute below export statements 
AKS_VNET=aks-vnet
AKS_VNET_ADDRESS_PREFIX=10.0.0.0/8
AKS_VNET_SUBNET_DEFAULT=aks-subnet-default
AKS_VNET_SUBNET_DEFAULT_PREFIX=10.240.0.0/16
AKS_VNET_SUBNET_VIRTUALNODES=aks-subnet-virtual-nodes
AKS_VNET_SUBNET_VIRTUALNODES_PREFIX=10.241.0.0/16

# Create Virtual Network & default Subnet
az network vnet create -g ${AKS_RESOURCE_GROUP} \
                       -n ${AKS_VNET} \
                       --address-prefix ${AKS_VNET_ADDRESS_PREFIX} \
                       --subnet-name ${AKS_VNET_SUBNET_DEFAULT} \
                       --subnet-prefix ${AKS_VNET_SUBNET_DEFAULT_PREFIX} 

# Create Virtual Nodes Subnet in Virtual Network
az network vnet subnet create \
    --resource-group ${AKS_RESOURCE_GROUP} \
    --vnet-name ${AKS_VNET} \
    --name ${AKS_VNET_SUBNET_VIRTUALNODES} \
    --address-prefixes ${AKS_VNET_SUBNET_VIRTUALNODES_PREFIX}

# Get Virtual Network default subnet id
AKS_VNET_SUBNET_DEFAULT_ID=$(az network vnet subnet show \
                           --resource-group ${AKS_RESOURCE_GROUP} \
                           --vnet-name ${AKS_VNET} \
                           --name ${AKS_VNET_SUBNET_DEFAULT} \
                           --query id \
                           -o tsv)
echo ${AKS_VNET_SUBNET_DEFAULT_ID}
```

#### Pre-requisite-3: Create Azure AD Group & Admin User
-   Create Azure AD Group: aksadmins
-   Create Azure AD User: aksadmin1 and associate to aksadmins ad group

```
# Create Azure AD Group
AKS_AD_AKSADMIN_GROUP_ID=$(az ad group create --display-name aksadmins --mail-nickname aksadmins --query objectId -o tsv)    
echo $AKS_AD_AKSADMIN_GROUP_ID

# Create Azure AD AKS Admin User 
# Replace with your AD Domain - aksadmin1@stacksimplifygmail.onmicrosoft.com
AKS_AD_AKSADMIN1_USER_OBJECT_ID=$(az ad user create \
  --display-name "AKS Admin1" \
  --user-principal-name aksadmin1@stacksimplifygmail.onmicrosoft.com \
  --password @AKSDemo123 \
  --query objectId -o tsv)
echo $AKS_AD_AKSADMIN1_USER_OBJECT_ID

# Associate aksadmin User to aksadmins Group
az ad group member add --group aksadmins --member-id $AKS_AD_AKSADMIN1_USER_OBJECT_ID

# Make a note of Username and Password
Username: aksadmin1@stacksimplifygmail.onmicrosoft.com
Password: @AKSDemo123
```
![](Pasted%20image%2020220917232807.png)

#### Pre-requisite-4: Create SSH Key
```
# Create Folder
mkdir $HOME/.ssh/aks-prod-sshkeys

# Create SSH Key
ssh-keygen \
    -m PEM \
    -t rsa \
    -b 4096 \
    -C "azureuser@myserver" \
    -f ~/.ssh/aks-prod-sshkeys/aksprodsshkey \
    -N mypassphrase

# List Files
ls -lrt $HOME/.ssh/aks-prod-sshkeys

# Set SSH KEY Path
AKS_SSH_KEY_LOCATION=/Users/kalyanreddy/.ssh/aks-prod-sshkeys/aksprodsshkey.pub
echo $AKS_SSH_KEY_LOCATION
```

-   Reference for [Create SSH Key](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/create-ssh-keys-detailed)

> To create the keys, a preferred command is `ssh-keygen`, which is available with OpenSSH utilities in the Azure Cloud Shell, a macOS or Linux host, and Windows (10 & 11).
> If an existing SSH key pair is found in the current location, those files are overwritten.

#### Pre-requisite-5: Create Log Analytics Workspace

```
# Create Log Analytics Workspace
AKS_MONITORING_LOG_ANALYTICS_WORKSPACE_ID=$(az monitor log-analytics workspace create               --resource-group ${AKS_RESOURCE_GROUP} \
                                           --workspace-name aksprod-loganalytics-workspace1 \
                                           --query id \
                                           -o tsv)

echo $AKS_MONITORING_LOG_ANALYTICS_WORKSPACE_ID
```

#### Pre-requisite-6: Get Azure AD Tenant ID and Set Windows Username Passwords

```
# List Kubernetes Versions available as on today
az aks get-versions --location ${AKS_REGION} -o table

# Get Azure Active Directory (AAD) Tenant ID
AZURE_DEFAULT_AD_TENANTID=$(az account show --query tenantId --output tsv)
echo $AZURE_DEFAULT_AD_TENANTID
or
Go to Services -> Azure Active Directory -> Properties -> Tenant ID


# Set Windows Server/Node Username & Password
AKS_WINDOWS_NODE_USERNAME=azureuser
AKS_WINDOWS_NODE_PASSWORD="P@ssw0rd1234"
echo $AKS_WINDOWS_NODE_USERNAME, $AKS_WINDOWS_NODE_PASSWORD
```

### Create Cluster with System Node Pool

```
# Set Cluster Name
AKS_CLUSTER=aksprod1
echo $AKS_CLUSTER

# Upgrade az CLI  (To latest version)
az --version
az upgrade

# Create AKS cluster 
az aks create --resource-group ${AKS_RESOURCE_GROUP} \
              --name ${AKS_CLUSTER} \
              --enable-managed-identity \
              --ssh-key-value  ${AKS_SSH_KEY_LOCATION} \
              --admin-username aksnodeadmin \
              --node-count 1 \
              --enable-cluster-autoscaler \
              --min-count 1 \
              --max-count 100 \
              --network-plugin azure \
              --service-cidr 10.0.0.0/16 \
              --dns-service-ip 10.0.0.10 \
              --docker-bridge-address 172.17.0.1/16 \
              --vnet-subnet-id ${AKS_VNET_SUBNET_DEFAULT_ID} \
              --enable-aad \
              --aad-admin-group-object-ids ${AKS_AD_AKSADMIN_GROUP_ID}\
              --aad-tenant-id ${AZURE_DEFAULT_AD_TENANTID} \
              --windows-admin-password ${AKS_WINDOWS_NODE_PASSWORD} \
              --windows-admin-username ${AKS_WINDOWS_NODE_USERNAME} \
              --node-osdisk-size 30 \
              --node-vm-size Standard_DS2_v2 \
              --nodepool-labels nodepool-type=system nodepoolos=linux app=system-apps \
              --nodepool-name systempool \
              --nodepool-tags nodepool-type=system nodepoolos=linux app=system-apps \
              --enable-addons monitoring \
              --workspace-resource-id ${AKS_MONITORING_LOG_ANALYTICS_WORKSPACE_ID} \
              --enable-ahub \
              --zones {1,2,3}
```

## Node pools
![](Pasted%20image%2020220724153227.png)

> **Windows Node Pool**
> Remember to mention the User name/Password for the windows node pool while creating the cluster.
> Otherwise you can't create or add a windows node pool later.
> Only way is to delete the cluster and recreate.
> **Virtual Node Pool**
> is of only linux and uses Azure container instances.

## Deploy apps to different node pools
![](Pasted%20image%2020220724153454.png)


# AKS - Multiple clusters
https://github.com/bhaaskara/azure-aks-kubernetes-masterclass/tree/master/21-Azure-AKS-Authentication-and-RBAC/21-01-AKS-Cluster-Access-Multiple-Clusters

```sh
# configure AKSDEM03 & 4 access
az aks get-credentials --resource-group aks-rg3 --name <aksdemo3(clustername)>
az aks get-credentials --resource-group aks-rg4 --name <aksdemo3(clustername)>
```

View kube config
`kubectl config view`

## Context
![](Pasted%20image%2020220802212151.png)

`kubectl config current-context` # View context
`kubectl config use-context aksdemo3` # Switch context

# AKS - Auto Scaling
- cluster auto scaler
- pod auto scaler - HPA

![](Pasted%20image%2020220806151855.png)

## Cluster AutoScaler
• Cluster Autoscaler is a tool that automatically adjusts the size of a Kubernetes cluster when one of the following conditions is true: 
• There are pods that failed to run in the cluster due to insufficient resources. 
• There are nodes in the cluster that have been underutilized for an extended period of time and their pods can be placed on other existing nodes. 
• The Cluster Autoscaler modifies our nodepools so that they scale out when we need more resources and scale in when we have underutilized resources. 

https://github.com/bhaaskara/azure-aks-kubernetes-masterclass/tree/master/22-Azure-AKS-Autoscaling

> Cluster auto scaling is per node pool.

# AKS - Storage
https://docs.microsoft.com/en-us/azure/aks/concepts-storage
https://docs.microsoft.com/en-us/azure/aks/operator-best-practices-storage

# AKS - Best practices for BCP and DR
https://docs.microsoft.com/en-us/azure/aks/operator-best-practices-multi-region

# AKS - Workload and API objects backup
https://docs.microsoft.com/en-us/azure-stack/aks-hci/backup-workload-cluster

# References
-   [**Azure AKS Kubernetes - Masterclass | Azure DevOps, Terraform:**](https://github.com/stacksimplify/azure-aks-kubernetes-masterclass) https://github.com/stacksimplify/azure-aks-kubernetes-masterclass
    
-   [**Azure DevOps for Kubernetes Workloads running on Azure AKS Cluster:**](https://github.com/stacksimplify/azure-devops-github-acr-aks-app1) https://github.com/stacksimplify/azure-devops-github-acr-aks-app1
    
-   [**Provision Azure AKS Cluster using Terraform and Azure DevOps:**](https://github.com/stacksimplify/azure-devops-aks-kubernetes-terraform-pipeline) https://github.com/stacksimplify/azure-devops-aks-kubernetes-terraform-pipeline
    
-   [**Docker Fundamentals:**](https://github.com/stacksimplify/docker-fundamentals) https://github.com/stacksimplify/docker-fundamentals
    
-   [**Presentation with 250 Slides outlining the various architectures and designs we are going to do in this course:**](https://github.com/stacksimplify/azure-aks-kubernetes-masterclass/tree/master/ppt-presentation) https://github.com/stacksimplify/azure-aks-kubernetes-masterclass/tree/master/ppt-presentation