# Create AKS cluster
Azure portal -> kubernetes services -> create kubernetes cluster

-   **Basics**
    -   **Subscription:** Free Trial
    -   **Resource Group:** Creat New: aks-rg1
    -   **Kubernetes Cluster Name:** aksdemo1
    -   **Region:** (US) Central US
    -   **Kubernetes Version:** Select what ever is latest stable version
    -   **Node Size:** Standard DS2 v2 (Default one)
    -   **Node Count:** 1
-   **Node Pools**
    -   leave to defaults
-   **Authentication**
    -   Authentication method: System-assigned managed identity
    -   Rest all leave to defaults
-   **Networking**
    -   **Network Configuration:** Advanced
    -   **Network Policy:** Azure
    -   Rest all leave to defaults
-   **Integrations**
    -   Azure Container Registry: None
    -   leave to defaults
-   **Tags**
    -   leave to defaults
-   **Review + Create**
    -   Click on **Create**

**_Why a managed Identity_**:
An Azure Kubernetes Service (AKS) cluster requires an identity to access Azure resources like load balancers and managed disks. This identity can be either a managed identity or a service principal. By default, when you create an AKS cluster a system-assigned managed identity automatically created. The identity is managed by the Azure platform and doesn't require you to provision or rotate any secrets.
Managed identities are essentially a wrapper around service principals, and make their management simpler.
To use a [service principal](https://docs.microsoft.com/en-us/azure/aks/kubernetes-service-principal), you have to create one, as AKS does not create one automatically. Clusters using a service principal eventually expire and the service principal must be renewed to keep the cluster working. Managing service principals adds complexity, thus it's easier to use managed identities instead.

Ref: https://docs.microsoft.com/en-us/azure/aks/use-managed-identity

# Connect to AKS Cluster
## Using Cloud Shell
-   Go to [https://shell.azure.com](https://shell.azure.com/)

```
# Connect to cluster
az aks get-credentials --resource-group <Resource-Group-Name> --name <Cluster-Name>
    # It creates ./kube/config file to connect to the cluster

# List Kubernetes Worker Nodes
kubectl get nodes 
kubectl get nodes -o wide

# List Namespaces
kubectl get namespaces
kubectl get ns

# List Pods from all namespaces
kubectl get pods --all-namespaces

# List all k8s objects from Cluster Control plane
kubectl get all --all-namespaces
```

## Desktop
```
# Install Azure CLI

# Login to Azure
az login

# Install Azure AKS CLI
az aks install-cli

# Configure Cluster Creds (kube config)
az aks get-credentials --resource-group <aks-rg1> --name <clustername>

# List AKS Nodes
kubectl get nodes 
kubectl get nodes -o wide
```

# AKS Trouble shooting
## Activity log
Azure portal -> AKS -> <cluster> -> Activity log