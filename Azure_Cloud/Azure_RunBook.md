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
