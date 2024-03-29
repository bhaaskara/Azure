# IAM
## Azure AD
### Create a New Azure AD tenant

## Create a user
Azure portal -> Azure AD -> Users
![](Pasted%20image%2020220712105940.png)

**Note:** Domain under user name should be the domain associated with the tenant.

## Create a Group
![](Pasted%20image%2020220712110215.png)
Membership type: 
assigned - Users are assigned to a group manually
Dynamic - Users added dynamically based on defined query.
![](Pasted%20image%2020220712110635.png)

## Self service password reset
enabling self service password will allow users to change their password with out admins intervention.

SSPR needs a Premium (p1/p2) subscription.

![](Pasted%20image%2020220712111050.png)

### SSPR settings
![](Pasted%20image%2020220712111242.png)
![](Pasted%20image%2020220712111334.png)

## Add device to Azure AD tenant
Adding device will allow access to resources like email, apps and the network.
Connecting means your work or school may control some settings on the device.
**Note:** Devices can't added from azure portal, they have to be added externally(i.e device has to initiate the connection).

## Bulk activities
![](Pasted%20image%2020220712113526.png)
Its new feature allows to do bulk operations.
Click on bulk invite -> download template (CSV) -> update -> upload -> invite bulk people to Azure AD.

# Compute
## VM
### Availability Set
**1. Placing a new virtual machine in the availability set**

We have already seen how virtual machines get placed in an availability set.

Below is a snapshot of what would happen if we placed another virtual machine in the availability set.

![](https://img-c.udemycdn.com/redactor/raw/2020-02-17_07-40-41-5b943f21c2cf9c7071eb85e97947fe44.jpg)

The availability set service would place the machine accordingly so that the virtual machines get placed across the various fault and update domains.

**2. What happens if we try to delete the availability set as it is?**

Would it delete the underlying virtual machines accordingly.

Well from the Azure portal , if we just try to delete the availability set as it is , we will get the following error message

![](https://img-c.udemycdn.com/redactor/raw/2020-02-17_07-43-26-702f6d15e977dbe50f94f56496157e83.jpg)

Hence we first have to ensure no virtual machine is associated with the availability set

So to delete the availability set, you can first delete the virtual machines linked to the availability set and then go ahead and delete the availability set.

#### Availability set use case
![](Pasted%20image%2020220720190222.png)
as the requirement is to have atleast 4 VMs during a planned maintenance we should have 5 update domains.
![](Pasted%20image%2020220720190343.png)

**Availability sets VS Availability zones**
AS will be useful to tolerate failures from with in the single DC.
AZ will be useful to tolerate failures from Data center failures.

# Storage
## Create a Storage account
![](Pasted%20image%2020220713124815.png)
Page blobs: VM Disks or Disks for SQL DB
![](Pasted%20image%2020220713124911.png)
![](Pasted%20image%2020220713125027.png)

## Object replication
### Create replication rule
![](Pasted%20image%2020220713121038.png)
![](Pasted%20image%2020220713121119.png)
Filters: is a prefix, and moves the files matching with the prefix.
            When not mentioned all the files are copied.
Copy over: Only new objects are copied over by default and can be changed to copy existing files.

**Note:** Azure uses blob change feed to monitor and track the changes to copy the files over.
          Turning off this feature will interrupt the replication.
          ![](Pasted%20image%2020220713121834.png)
  Ref: LAB: https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/blob/master/Instructions/Labs/LAB_07-Manage_Azure_Storage.md

## Create Azure file share

# Networking
## Create a VNET
Portal -> Virtual networks
![](Pasted%20image%2020220714121418.png)
![](Pasted%20image%2020220714121608.png)
So this VNET can have 255 subnets with 251 IPs in each subnet.

VNET with multiple addresses spaces
![](Pasted%20image%2020220714122843.png)

Add a subnet
![](Pasted%20image%2020220714121813.png)


## Create Static Public IP

```
# Get the resource group name of the AKS cluster 
az aks show --resource-group aks-rg1 --name aksdemo1 --query nodeResourceGroup -o tsv

# TEMPLATE - Create a public IP address with the static allocation
az network public-ip create --resource-group <REPLACE-OUTPUT-RG-FROM-PREVIOUS-COMMAND> --name myAKSPublicIPForIngress --sku Standard --allocation-method static --query publicIp.ipAddress -o tsv

# REPLACE - Create Public IP: Replace Resource Group value
az network public-ip create --resource-group MC_aks-rg1_aksdemo1_centralus --name myAKSPublicIPForIngress --sku Standard --allocation-method static --query publicIp.ipAddress -o tsv
```

## Load balancer
### Basic Load balancer
**Basic load balancer dont allow individual VMs which are not part of availabilty set**

### Standard Public load balancer
#### Create a public load balancer
https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal
az-104 dyan account - vid 130

#### Add backend pool to the public load balancer

![[Pasted image 20220910225101.png]]
![[Pasted image 20220910225430.png]]
![[Pasted image 20220910225708.png]]
![[Pasted image 20220910225801.png]]
**Standard load balancer allow individual VMs not oart of an availability set**
**Basic load balancer dont allow individual VMs which are not part of availabilty set**

#### Add health probe
![[Pasted image 20220910230014.png]]
![[Pasted image 20220910230251.png]]
#### Add load balancing rules
![[Pasted image 20220910230329.png]]
![[Pasted image 20220910230402.png]]
![[Pasted image 20220910230446.png]]
![[Pasted image 20220910230509.png]]
### Azure App Gateway
vid 139-143 - Azure 104 training 

### Azure Traffic Manager

Azure portal -> traffic manager -> Create
![](Pasted%20image%2020220809173400.png)
Routing Methods
![](Pasted%20image%2020220809173323.png)

Add endpoint
(Create the 2 webapps to be configured as end points)
![](Pasted%20image%2020220809173532.png)

## VNET Peering
- Virtual Network Peering is used to connect two Azure virtual networks together via the backbone network.
- Azure supports connecting two virtual networks located in the same region or networks located across regions.
- Once you enable virtual network peering between two virtual networks, the virtual machines can then communicate via their private IP addresses across the peering connection.
- You can also peer virtual networks that are located across different subscriptions.
- The virtual networks can't have overlapping CIDR blocks.

# Azure WebApp (PAAS)
## Publish to an Azure WebApp
1. Create a webApp resource in portal
2. Publish your app (code) from VS to the webapp
3. Access the App through URL


**1. Create Azure WebApp Resource**
Portal -> Azure Webapp -> Create
![](Pasted%20image%2020220814191640.png)
![](Pasted%20image%2020220814191743.png)

Select the App Service plan
![](Pasted%20image%2020220814191718.png)

**3. Access App through URL**
![](Pasted%20image%2020220814192015.png)


# Trouble Shooting
## Troubleshoot Azure VM connectivity problems
https://docs.microsoft.com/en-us/azure/virtual-network/troubleshoot-vm-connectivity?source=recommendations

## No Internet access from Azure Windows VM that has multiple IP addresses
https://docs.microsoft.com/en-us/troubleshoot/azure/virtual-machines/no-internet-access-multi-ip?source=recommendations