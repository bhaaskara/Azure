# Infra
## Availability set, fault domain and update domain
https://www.c-sharpcorner.com/article/availability-set-fault-domains-and-update-domains-in-azure-virtual-machie/

## Resource Groups
A way of organizing resources in a subscription
A folder structure
All resources must belong to only one resource group
Resource groups can be deleted (which deletes the resources inside)
A way to separating out projects, keeping unrelated things separate

![](Pasted%20image%2020220712130405.png)
RGs will have a location, but its logical and the resources in it can be from any location.

## RG Lock
Azure portal -> RG -> Locks
Adding a lock prevents users from doing the activity.

## Management Groups
If your organization has many Azure subscriptions, you may need a way to efficiently manage access, policies, and compliance for those subscriptions. _Management groups_ provide a governance scope above subscriptions. You organize subscriptions into management groups the governance conditions you apply cascade by inheritance to all associated subscriptions.

However, all subscriptions within a single management group must trust the same Azure Active Directory (Azure AD) tenant.

For example, you can apply policies to a management group that limits the regions available for virtual machine (VM) creation. This policy would be applied to all nested management groups, subscriptions, and resources, and allow VM creation only in authorized regions.

## Azure policy
Azure Policy helps to enforce organizational standards and to assess compliance.

### Built-in policies
- Require SQL Server 12.0
- Allowed Storage Account SKUs
- Allowed Resource Type
- Allowed Locations
- Allowed Virtual Machine SKUs
- Apply tag and its default value
- Enforce tag and its value
- Not allowed resource types

Policies can be assigned to RGs, Subscription or Individual resources.

**Note:** after applying the policy if its state is not started, then start it from Powershell / CLI
```
start-azPolicyComplianceScan -ResourceGroupname `newrg`
#it akes some time to get it compliant or not.
```

# Account and subscription
![](Pasted%20image%2020220608192612.png)

**Account**
The Azure account is a global unique entity that gets you access to Azure services and
your Azure subscriptions. You can create multiple subscriptions in your Azure account to
create separation e.g. for billing or management purpose, manage resources In resource groups.

**Subscription**
- Logical unit of Azure services that is linked to an Azure account.
- Security and billing boundary.

**Subscription Types**
- Free
- Pay-as-you-go
- Enterprise

# IAM
Azure provides an identity management system based on their popular "Active directory".
Azure active directory is not the same as Active Directory.

## Windows AD vs Azure AD
![](Pasted%20image%2020220613114538.png)
- A cloud-based suite of identity management capabilities that enables you to
   securely manage access to Azure services and resources for your users .
- Provides application management, authentication, device management, and
   hybrid identity
- Traditional Active directory does not work with internet protocols.
- SAML, Oauth and OpenID are authentication protocols.

## Azure AD
![](Pasted%20image%2020220613114841.png)

## Azure AD connect
![](Pasted%20image%2020220613115003.png)

Integrates on-premises AD with Azure AD.

## Azure AD and Subscription
![](Pasted%20image%2020220613115440.png)
- An Azure subscription has a trust relationship with Azure Active Directory (Azure AD). 
- A subscription trusts Azure AD to authenticate users, services, and devices.
- Multiple subscriptions can trust/associate with the same Azure AD directory, but one subscription can only associate with a single directory.

## Basic concepts of Account and subscriptions
### Account / User
A person or a program

• Person - i.e. Joe Smith - ioe@example.com
   User name, password, multi-factor authentication
- App - Managed Identity
  Represents a program or service

### Tenant
A representation of an organization
Usually represented by a public domain name - i.e. example.com
Will be assigned a domain if not specified - i.e. example.onmicrosoft.com
A dedicated instance of Azure Active Directory
Every Azure Account is part of at least one tenant

Check your tenant details under user
![](Pasted%20image%2020220712121719.png)

### Subscription
An agreement with Microsoft to use Azure services, and how you're going to pay for that
All Azure resource usage gets billed to the payment method of the subscription
- Free subscriptions
- Pay as you go
- Enterprise agreements

**Note:** Not every tenant need to have a subscription.
                So tenant with out a subscription can not create any resources.
          Tenant can have multiple subscriptions
          More than one account can be the owner in a tenant.

## RBAC
In Azure authentication is managed by Azure AD and authorization is managed by RBAC Roles.

### Roles and Administrators
Every Azure AD account(tenant) will have a global admin.
Depending on the requirement you can assign roles from roles menu, Users menu or Groups menu.

ex: Azure portal -> AD account -> Users -> select user -> assign a role

Roles can be assigned from Resource group as well.
Azure portal -> RG -> IAM -> Role assignment

**Default types of roles in RGs**
Owner - can work with resources in RG and grant permission to other users to work on RG
Contributor - can work (read and update) with resources but can't grant permissions to other users
Reader - only view access on RG

**Note:** Role can be assigned to a user or group at individual resource level, RG or Subscription level. To do so go to that perticular resource in Azure portal -> IAM and assign a role.

### Custom roles
Custom roles need Azure AD Premium (P1/P2) subscription.

Custom roles can be created from Azureportal -> Azure AD -> IAM
or Portal -> RG -> IAM -> Custom roles

# Azure Storage
![](Pasted%20image%2020220712154453.png)
Blob is an object storage
    images
    movies
File share
    NFS
    CIFS
Disk is a block storage

## Azure storage account
An Azure storage account contains all of your Azure Storage data objects: blobs, files, queues, tables, and disks. The storage account provides a unique namespace for your Azure Storage data that is accessible from anywhere in the world over HTTP or HTTPS. Data in your Azure storage account is durable and highly available, secure, and massively scalable.

## Types of storage accounts
![](Pasted%20image%2020220712155415.png)
![](Pasted%20image%2020220712155447.png)
Note: Table storage here doesn't refer to SQL data. 

## VM and Storage
![](Pasted%20image%2020220712155627.png)

![](Pasted%20image%2020220712155651.png)

## Redundancy
### LRS - Local redundant storage
Azure keeps 3 copies of data in the same location for redundancy and provides 11 9's durability.

### GRS - Geo redundant storage
Azure keeps 6 copies of data in two different regions (3 copies in each region) for redundancy.

You can failover the data under Portal -> storage account -> data management -> Geo replication.

## Access Tier
- Hot
    - Cost more for storage and less for access
- Cool
    - Cost less for storage (50% cheaper than hot) and more for access (costs per read/write)
    - Data must remain in cool tier for at least 30 days or be subject to an early deletion charge.
    - This can be set at container level or per file
- Archive
    - Cost very less for storage (90% cheaper than hot) and way higher for access
    - This is suited for files that almost never used.
    - Data must remain in archive tier for at least 180 days or be subject to an early deletion charge. 

## Lifecycle Management
Portal -> Storage accounts -> Blob service -> Lifecycle management

Azure Storage lifecycle management offers a rule-based policy that you can use to transition blob data to the appropriate access tiers or to expire data at the end of the data lifecycle.

Apply rules to containers or to a subset of blobs, using name prefixes or [blob index tags](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-manage-find-blobs) as filters.

## Access control
### Access keys and SAS
**Access Keys** gives full access to the storage resource.
**Shared access signature** allows more fine grained access with expiry date.

### IAM
Create a role and assign the roles(permissions) to the individual users, groups or System managed identities.(app service, virtual machine)

## Object replication
Portal -> Storage account -> blob services -> object replication

When object replication is enabled, blobs are copied —from a source storage account to a destination account. Cross - tenant policies will show up under "Other accounts". The storage accounts may be in different Azure regions.

## Azure Disks
• Azure Disk Storage offers high-performance, highly durable block storage
   for our mission- and business-critical workloads
• We can mount these volumes as devices on our Virtual Machines & Container 
    instances.
• Cost-effective storage
    • Built-in bursting capabilities to handle unexpected traffic and process batch 
       jobs
• Unmatched resiliency
    • 0 percent annual failure rate for consistent enterprise-grade durability
• Seamless scalability
    • Dynamic scaling of disk performance on Ultra Disk Storage without disruption
• Built-in security
    • Automatic encryption to help protect your data using Microsoft-managed keys 
       or your own
