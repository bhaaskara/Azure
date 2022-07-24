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
- Integrated security with Azure Active Directory (Azure AD) authentication, role-based access control, Docker Content Trust and virtual network 
integration.

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
![](Pasted%20image%2020220723161207.png)

https://github.com/bhaaskara/azure-aks-kubernetes-masterclass/tree/master/21-Azure-AKS-Authentication-and-RBAC

# AKS Autoscaling
https://github.com/bhaaskara/azure-aks-kubernetes-masterclass/tree/master/22-Azure-AKS-Autoscaling
