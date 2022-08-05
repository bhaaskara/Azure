# Why do we need Ingress?

![](Pasted%20image%2020220710221407.png)

# What is Ingress
Ingress is a Kubernetes resource that lets us configure an HTTP load balancer for applications running
on Kubernetes, with advanced capabilities at HTTP layer.

# Ingress features
- context path based routing
- host based routing
- SSL Termination

# Ingress Terminology
## Ingress controller
- Ingress controller is an application that runs in a cluster and configures an HTTP load balancer according to Ingress resources.
- The load balancer can be a software load balancer running in the cluster or a hardware or cloud load balancer running externally.
- In the case of NGINX, the Ingress controller is deployed in a pod along with the load balancer.

## Ingress resource or service
- Equivalent to Kubernetes Services but with lot of additional features
- We can define routing rules based on URI or Hostname in Ingress Resource Manifest
- We can even define SSL/ TLS related information in Ingress Resource Manifest.

# Azure AKS and NGINX ingress controller
## Context Path based routing
![](Pasted%20image%2020220711143536.png)



## SSL 
![](Pasted%20image%2020220711144058.png)

**Note:** Azure AGIC (Application gateway ingress controller) is Azure's ingress controller.

# Implement NGINX ingress controller in Azure AKS
-   We are going to create a **Static Public IP** for Ingress in Azure AKS
-   Associate that Public IP to **Ingress Controller** during installation.
-   We are going to create a namespace `ingress-basic` for Ingress Controller where all ingress controller related things will be placed.
-   In future, we install **cert-manager** for SSL certificates also in same namespace.
-   **Caution Note:** This namespace is for Ingress controller stuff, ingress resource we can create in any other namespaces and not an issue. Only condition is create ingress resource and ingress pointed application in same namespace (Example: App1 and Ingress resource of App1 should be in same namespace)
-   Create / Review Ingress Manifest
-   Deploy a simple Nginx App1 with Ingress manifest and test it
-   Clean-Up or delete application after testing

## Step1: Create Static Public IP

```
# Get the resource group name of the AKS cluster 
az aks show --resource-group aks-rg1 --name aksdemo1 --query nodeResourceGroup -o tsv

# TEMPLATE - Create a public IP address with the static allocation
az network public-ip create --resource-group <REPLACE-OUTPUT-RG-FROM-PREVIOUS-COMMAND> --name myAKSPublicIPForIngress --sku Standard --allocation-method static --query publicIp.ipAddress -o tsv

# REPLACE - Create Public IP: Replace Resource Group value
az network public-ip create --resource-group MC_aks-rg1_aksdemo1_centralus --name myAKSPublicIPForIngress --sku Standard --allocation-method static --query publicIp.ipAddress -o tsv
```

-   Make a note of Static IP which we will use in next step when installing Ingress Controller

```
# Make a note of Public IP created for Ingress
52.154.156.139
```

# External DNS with Ingress (Host name based routing and External DNS)
• ExternalDNS synchronizes exposed Kubernetes Services and Ingresses with DNS provider*
• In simple terms, ExternalDNS allows you to control DNS records dynamically via Kubernetes resources in a DNS provider-agnostic way.
• Reference: [https://github.com/kubernetes-sigs/external-dns](https://github.com/kubernetes-sigs/external-dns)

> Whenever you mention the host name in Ingress its automatically gets created/synchronized with the Azure DNS Zones (by automatically creating the record sets).

![](Pasted%20image%2020220801235739.png)

- Virtual machine scale set in AKS cluster which is hosting the work load pods (external DNS, Ingress and apps) accesses the DNS zones through managed service identity.
- once the host name (eapp1.kubeoncloud.com) defined in ingress service it triggers the external DNS which is hosted on virtual machine scale set will create/update/delete the entries in DNS zones through MSI.
- External DNS is a k8s addon and will be deployed on the k8s.