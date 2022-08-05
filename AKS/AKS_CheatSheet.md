# Create AKS cluster - CLI
-   Generates SSH Keys with option **--generate-ssh-keys**
-   They will be stored in **$HOME/.ssh** folder in your local desktop
-   Backup them if required

```
# Create AKSDEMO3 Cluster
az group create --location centralus --name aks-rg3
az aks create --name aksdemo3 \
              --resource-group aks-rg3 \
              --node-count 1 \
              --enable-managed-identity \
              --generate-ssh-keys

# Backup SSH Keys
cd $HOME/.ssh
mkdir BACKUP-SSH-KEYS-AKSDEMO-Clusters
cp id_rsa* BACKUP-SSH-KEYS-AKSDEMO-Clusters
ls -lrt BACKUP-SSH-KEYS-AKSDEMO-Clusters
```

## Use existing SSH keys for cluster creation using **--ssh-key-value**
```
# Create AKSDEMO4 Cluster
az group create --location centralus --name aks-rg4
az aks create --name aksdemo4 \
              --resource-group aks-rg4 \
              --node-count 1 \
              --enable-managed-identity \
              --ssh-key-value /Users/kalyanreddy/.ssh/id_rsa.pub  
```

# Access the AKS Cluster
```
# View kubeconfig
kubectl config view

# Clean existing kube configs
cd $HOME/.kube
>config
cat config

# View kubeconfig
kubectl config view

# Configure AKSDEMO3 & 4 Cluster Access for kubectl
az aks get-credentials --resource-group aks-rg3 --name aksdemo3

# View kubeconfig
kubectl config view

# View Cluster Information
kubectl cluster-info

# View the current context for kubectl
kubectl config current-context
```

# Configure another cluster access to kubectl
```
# Configure AKSDEMO4 Cluster Access for kubectl
az aks get-credentials --resource-group aks-rg4 --name aksdemo4

# View the current context for kubectl
kubectl config current-context

# View Cluster Information
kubectl cluster-info

# View kubeconfig
kubectl config view
```

# Switch context between clusters
```
# View the current context for kubectl
kubectl config current-context

# View kubeconfig
kubectl config view 
Get contexts.context.name to which you want to switch 

# Switch Context
kubectl config use-context aksdemo3

# View the current context for kubectl
kubectl config current-context

# View Cluster Information
kubectl cluster-info
```