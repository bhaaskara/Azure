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

### RG Lock
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
It takes 30min for the policy to be affective.

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
- Multiple subscriptions can trust/associate with the same Azure AD directory, but one subscription can only associate with one directory.

## Basic concepts of Account and subscriptions
### Account / User
A person or a program

• Person - i.e. Joe Smith - ioe@example.com
   User name, password, multi-factor authentication
- App - Managed Identity
  Represents a program or service

### Tenant
A dedicated instance of Azure Active Directory
A representation of an organization
Usually represented by a public domain name - i.e. example.com
Will be assigned a domain if not specified - i.e. example.onmicrosoft.com
Every Azure Account is part of at least one tenant

Check your tenant details under user
![](Pasted%20image%2020220712121719.png)

### Subscription
An agreement with Microsoft to use Azure services, and how you're going to pay for that.
All Azure resource usage gets billed to the payment method of the subscription.
- Free subscriptions
- Pay as you go
- Enterprise agreements

**Note:** Not every tenant need to have a subscription.
                So tenant with out a subscription can not create any resources.
          Tenant can have multiple subscriptions
          More than one account can be the owner in a tenant.
          The `Usage + quota` under subscription will show the resources you can create under your subscription.

## RBAC
In Azure, authentication is managed by Azure AD and authorization is managed by RBAC Roles.

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

**Note:** Role can be assigned to a user or group at individual resource level, RG or Subscription level. To do so go to that particular resource in Azure portal -> IAM and assign a role.

### Custom roles
Custom roles need Azure AD Premium (P1/P2) subscription.

Custom roles can be created from Azureportal -> Azure AD -> IAM
or Portal -> RG -> IAM -> Custom roles

## Ref:
https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/

# Azure Compute 
## VMs
### Azure Disks for VMs
- Azure managed disks
- 99.999% availability
- Support with features such as Availability zones, Azure backup etc..

**Data Disk Types**
**Standard HDD**
- Ideal for backup environment and non-critical workloads.
- Max size - 32,767 GiB
- Max through put 500 MBPS
- Max IOPS 2000

**Standard SSD**
This is ideal for Web Servers 
and Dev/Test Environments. 
Max disk size - 32,767 GiB 
Max throughput - 750 MB/s 
Max lops - 6000. 

**Premium SSD**
This is ideal for Production 
environments. 
Max disk size - 32,767 GiB 
Max throughput — 950 MB'S 
Max lops - 20,000. 

**Note:** Not all VM sizes support premium disks.
          Check it under VM sizes

**Ultra disk**
This is ideal for 10 intensive workloads — SQL , Oracle databases. 
Max disk size — 65,536 GiB 
Max throughput — 4000 MB/s 
Max lops -160,000. 

### Attach a data disk to a VM
1. Create and attach a data disk in azure portal
2. on the windows VM -> server manager -> initialize new volume -> create new volume

### Azure Disk encryption
By default the OS Disks and Data disks are encrypted with SSE (Server Side Encryption) with PMK (Platform Managed Key).
if you want to encrypt the disks at OS level (DM crypt for Linux and Bitlocker for Windows) you can use ADE with User keys.
It uses Keys from Azure Key vault for user keys.
    you have to import or create a key in Key valut
    Assign the policy for Vm encryption to access key valut

### IOPS and Through put
IOPS - input/output operations per second
           DBs will have lot of IOPS
Through put - Amount of Data being sent to disk in specified interval
                       Video streaming apps will have lot of data being sent/read.

### Azure Shared Disks
This allows a managed disk to be attached to multiple virtual machines.
Used for clustered SQL server workloads
There are restrictions, can only be enable for Premium and Ultra disks.

**Note:** Create a manged disk and enable sharing in advanced options
           Attach the disk on to VM
           and Before attaching the shared disk to another VM, the first VM should be stopped.
           Need MPIO software to manage the shared disks

### Unmanaged Disks
Unmanaged disks nothing but your Azure storage service (i.e blob storage, file shares etc)
Unmanaged disks are charged per usage (i.e thin volumes).
VM can't have both managed and unmanaged disks
Need storage account to use unmanaged disks.

Once you enable unmanaged disks in Vm -> Storage -> advanced options
your OS disk will be stored in storage account.
The Storage account type and OS disk type should match.

### Custom script extensions
33,34 - # [AZ-104 Microsoft Azure Administrator Certification 2022](https://www.udemy.com/course/microsoft-certified-azure-administrator/)


### Linux VM - Custom data and Cloud Init
Azure -> VM -> Advanced options
![](Pasted%20image%2020220720131125.png)
Note: allow port 80 (required ports depend on the softwares) in NSG.

### Boot Diagnostics
Azure -> VM -> Support + trouble shooting

Helps in finding out the boot isues.
Shows system screen shot.
View and download the serial log.

### Serial console
Azure -> VM -> Serial console

Allows the serial console access to the VM.
Need custom boot diagnostics storage account.

To login use
```sh
cmd       # Create cmd session
ch -si 1  # connect to that session
```

### Run command
![](Pasted%20image%2020220720132509.png)
![](Pasted%20image%2020220720132539.png)

### Azure Confidential Computing

This is a feature that allows you to isolate sensitive data when it is being processed in the cloud.

This feature is available for your virtual machines. In Confidential computing , a part of the CPU’s hardware is reserved for the portion of code and data in your application. This portion is known as an enclave.

There is a special series of virtual machines which support confidential computing. This is the DCsv2-Series

[https://docs.microsoft.com/en-us/azure/virtual-machines/dcv2-series](https://docs.microsoft.com/en-us/azure/virtual-machines/dcv2-series)

These series of virtual machines are built on Intel SGX technology for hardware-based enclaves.

To actually ensure that your code or application runs inside the enclave, you will have to program it accordingly.

For this you need to use two open-source frameworks

a) Open Enclave Software Development Kit

[https://github.com/openenclave/openenclave](https://github.com/openenclave/openenclave)

·b)Confidential Consortium Framework

[https://github.com/Microsoft/CCF](https://github.com/Microsoft/CCF)

 ### Azure Dedicated Hosts

This service provides physical servers to host virtual machines. The physical server is dedicate to the Azure subscription.

The benefits of Azure Dedicated Hosts is that no other virtual machines from any other customers would be placed on the physical server.

You can also control the maintenance events that are initiated on the Azure platform.

Here the users are charged per dedicated host. This is irrespective of the number of virtual machines running on the physical server

For more information on Azure Dedicated Hosts , you can visit the URL - [https://docs.microsoft.com/en-us/azure/virtual-machines/windows/dedicated-hosts](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/dedicated-hosts)

### Azure Bastion service
![](Pasted%20image%2020220720133235.png)
Bastion subnet should be named as AzureBastionSubnet

Azure -> Vm -> Connect -> Bastion
if it not exists it ask you to create one.
now you can connect to your VM via browser with out public ip for a VM.

### Reset Password
Azure -> VM -> support and trouble shooting -> reset password

### Redeploy and Reapply
Azure -> VM -> support and trouble shooting -> redeploy + reapply
![](Pasted%20image%2020220720144930.png)

### Availability Sets
When you host your virtual machines in Azure, you sometimes need to cater to the following

1.  An unplanned event wherein the underlying infrastructure fails unexpectedly. The failures could be attributed to network failures , local disk failures or even rack failures.
2.  Planned maintenance events , wherein Microsoft needs to make planned updates to the underlying physical environment. In such cases , a reboot might be required on your virtual machine.
    
You can increase the availability of your application by making use of availability sets. Each virtual machine that is assigned to the availability set is assigned a separate fault and update domain.

**Fault domains** are used to define the group of virtual machines that share a common source and network switch. You can have up to 3 fault domains.

**Update domains** are used to group virtual machines and physical hardware that can be rebooted at the same time. You can have up to 20 update domains.

**Note:** VMs can be added to the availability set only while creating them.
           There is no extra cost involved with Availability sets.
           ![](Pasted%20image%2020220720194020.png)
           
### Availability Zones
-   This features help provides better availability for your application by protecting them from datacenter failures.
-   Each Availability zone is a unique physical location in an Azure region.
-   Each zone comprises of one or more data centers that has independent power, cooling, and networking
-   Hence the physical separation of the Availability Zones helps protect applications against data center failures
-   Using Availability Zones, you can be guaranteed an availability of 99.99% for your virtual machines. You need to ensure that you have 2 or more virtual machines running across multiple availability zones.
- There is extra for communication bandwidth across the VMs across the zones.

### Scale sets
- Scale set scale out/scale in the number of VMs based on rules
- Its user responsibility to configure the app after the VMs scale out.

## Proximity Placement Groups
![](Pasted%20image%2020220720201854.png)

![](Pasted%20image%2020220720201952.png)

## App service
## Functions logic app
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

## Import and export data to Azure
- AZCopy - CLI tool to copy data to azure storage account
- Storage Explorer - GUI tool to manage your azure storage account

### Import / Export large data
Portal -> Create Import / Export job
![](Pasted%20image%2020220713123912.png)
![](Pasted%20image%2020220713124010.png)
![](Pasted%20image%2020220713124114.png)
Azure ships a Hard disk with data

## Azure File Share
Note: Azure file share uses port 445 to transfer the data, and most of the ISPs blok this port on home computers due to security concerns. So you can use File Sync. 
And you should be able to mount the File Share on your office laptops.

### Azure File Sync
Azure File Sync Is a service that helps in syncing the files from your computer and Azure File share.

1. Download the Azure file sync client for your computer
2. Create File sync service in portal
3. Create a sync group
4. Register your server on Azure File Sync

Ref: https://docs.microsoft.com/en-us/azure/storage/file-sync/file-sync-troubleshoot?tabs=portal1%2Cazure-portal


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


# Backups
Portal -> Backup and site recovery (OMS) -> Create -> Recovery services vault

![](Pasted%20image%2020220713225053.png)
Note: : Location should be same as the resource location.
            when you want to take a backup of resources in different regions you have to have recovery service vault per region.

It contains two things
- backup
- site recovery - works as DR solution, if a disaster happens you can restore on to a different region.

## Create a backup
![](Pasted%20image%2020220713230434.png)
![](Pasted%20image%2020220713225622.png)

## VM backup and Restore a file from Backup
![](Pasted%20image%2020220713230242.png)
To recover a single file, goto file recovery and select the snapshot, download the tool that mounts that backup as drive on the VM, then you can browse and recover a file.

## On-prem backup
![](Pasted%20image%2020220713230457.png)

![](Pasted%20image%2020220713230523.png)
![](Pasted%20image%2020220713230553.png)
![](Pasted%20image%2020220713230654.png)

![](Pasted%20image%2020220713230738.png)

## Soft delete
Recovery Services Vault
- Enabled by default on new RSV
- 14 days to delete backup data
- Can be un-deleted
- 15th day - auto delete

## Azure Site Recovery
![](Pasted%20image%2020220714000405.png)
### Setup DR for a VM
![](Pasted%20image%2020220714000928.png)
Advanced settings
![](Pasted%20image%2020220714001128.png)

Note: It takes 15-30 min to show the progress.

and The backup and recovery jobs can be seen under recovery services vault too
![](Pasted%20image%2020220714001514.png)

you can see the events under
portal -> Site Recovery Vault -> monitoring -> recovery jobs / Backup jobs

you can see the progress under
portal -> Site Recovery Vault -> protected items -> replicated items / backup items

Ref: https://docs.microsoft.com/en-us/azure/backup/quick-backup-vm-portal

# Azure Networking
![](Pasted%20image%2020220714101035.png)

![](Pasted%20image%2020220714101853.png)
## VNet
Azure Virtual Network (VNet) is the fundamental building block for your private network in Azure.

Azure virtual network enables Azure resources to securely communicate with each other, the internet, and on-premises networks. Key scenarios that you can accomplish with a virtual network include - communication of Azure resources with the internet, communication between Azure resources, communication with on-premises resources, filtering network traffic, routing network traffic, and integration with Azure services.

### Communicate with the internet
All resources in a VNet can communicate outbound to the internet, by default. You can communicate inbound to a resource by assigning a public IP address or a public Load Balancer. You can also use public IP or public Load Balancer to manage your outbound connections.

```
Azure provides a default outbound access IP for VMs that either aren't assigned a public IP address or are in the back-end pool of an internal basic Azure load balancer. The default outbound access IP mechanism provides an outbound IP address that isn't configurable.

For more information, see [Default outbound access in Azure](https://docs.microsoft.com/en-us/azure/virtual-network/ip-services/default-outbound-access).

The default outbound access IP is disabled when either a public IP address is assigned to the VM or the VM is placed in the back-end pool of a standard load balancer, with or without outbound rules. If an [Azure Virtual Network network address translation (NAT)](https://docs.microsoft.com/en-us/azure/virtual-network/nat-gateway/nat-overview) gateway resource is assigned to the subnet of the virtual machine, the default outbound access IP is disabled.

VMs that are created by virtual machine scale sets in flexible orchestration mode don't have default outbound access.

For more information about outbound connections in Azure, see [Use source network address translation (SNAT) for outbound connections](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-outbound-connections).
```

## CIDR Notation
CIDR notation is a simpler notation for a network with subnet mask
- CIDR 192.0.2.0/24 means 192.0.2.0  with subnet mask 255.255.255.0  
   24 bits are masked and only last octet can change
   This network can have 254 IPs
- CIDR 192.0.2.0/16 means 192.0.2.0 with subnet mask 255.255.0.0
  16 bits are mashed and last two octets can change
   This network can have 256 subnets with 254 IPs in each subnet

Note: In a network the first IP and last IP are reserved for Network ID and Broadcast ID.
          so in 192.0.2.0/24 the available IPs are 254.

## Subnet
A subnet is a range of IP addresses in the virtual network. You can divide a virtual network into multiple subnets for organization and security. Each NIC in a VM is connected to one subnet in one virtual network. NICs connected to subnets (same or different) within a virtual network can communicate with each other without any extra configuration.

There aren't security boundaries by default between subnets. Virtual machines in each of these subnets can communicate. If your deployment requires security boundaries, use **Network Security Groups (NSGs)**, which control the traffic flow to and from subnets and to and from VMs.

VNET with single subnet
![](Pasted%20image%2020220714130302.png)

VNET with multiple subnets
![](Pasted%20image%2020220714130422.png)


![](Pasted%20image%2020220714121144.png)

## Network interfaces
A [network interface (NIC)](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface) is the interconnection between a virtual machine and a virtual network. A virtual machine must have at least one NIC. A virtual machine can have more than one NIC, depending on the size of the VM you create. 

You can create a VM with multiple NICs, and add or remove NICs through the lifecycle of a VM. Multiple NICs allow a VM to connect to different subnets.

Each NIC attached to a VM must exist in the same location and subscription as the VM. Each NIC must be connected to a VNet that exists in the same Azure location and subscription as the NIC. You can change the subnet a VM is connected to after it's created. You can't change the virtual network. Each NIC attached to a VM is assigned a MAC address that doesn't change until the VM is deleted.

- Secondary NIC has to be in the same region and VNET
- Secondary NIC can't be attached to a running VM

## NAT Gateway
Simplify connectivity to the internet using a NAT (network address translation) gateway, Internet (Outbound) connectivity is possible without a load balancer or public IP addresses attached to your virtual machines. 

## Azure Bastion
Azure Bastion is deployed to provide secure management connectivity to virtual machines in a virtual network. Azure Bastion Service enables you to securely and seamlessly RDP & SSH to the VMs in your virtual network. Azure bastion enables connections without exposing a public IP on the VM. Connections are made directly from the Azure portal, without the need of an extra client/agent or piece of software.

## NSG
NSG works as firewall
It can be attached to NIC, Subnet or both.

## Azure Load Balancer
Azure Load Balancer has 3 SKUs (stock keeping unit)- Basic, Standard, and Gateway. 
Each SKU is catered towards a specific scenario and has differences in scale, features, and pricing.

By default, the Standard SKU is used when you create an Azure Kubernetes Service(AKS) cluster

Basic load balancer is a Layer 4 load balancer, standard load balancer is L7 (application layer) LB.

![](Pasted%20image%2020220714223804.png)

### Basic LB - NAT Rules
Allows to connect (RDP/SSH) to the backend VMs (without public IP) through LBs public IP.

122-135 videos in # [AZ-104 Microsoft Azure Administrator Certification 2022](https://www.udemy.com/course/microsoft-certified-azure-administrator/) in Dyan's account.


## Ref: 
### VM with Multiple IP Addresses 
https://docs.microsoft.com/en-us/azure/virtual-network/ip-services/virtual-network-multiple-ip-addresses-portal

# Azure Monitor

The following diagram gives a high-level view of Azure Monitor. At the center of the diagram are the data stores for metrics and logs, which are the two fundamental types of data used by Azure Monitor. On the left are the [sources of monitoring data](https://docs.microsoft.com/en-us/azure/azure-monitor/agents/data-sources) that populate these [data stores](https://docs.microsoft.com/en-us/azure/azure-monitor/data-platform). On the right are the different functions that Azure Monitor performs with this collected data. Actions include analysis, alerting, and streaming to external systems.
![](Pasted%20image%2020220714231830.png)

![](Pasted%20image%2020220714231935.png)
Alerts can be configured based on the Metrics or Activity logs or Log analytics data.

Diagnostic settings
![](Pasted%20image%2020220714232838.png)

create alerts - 273 -  [AZ-104 Microsoft Azure Administrator Certification 2022](https://www.udemy.com/course/microsoft-certified-azure-administrator/) in Dyan's account.

## Log analytics workspace
![](Pasted%20image%2020220714233448.png)
To get the logs from a VM
- Create log analytics workspace
- Connect the VM
- to connect a individual server download and install the agent and configure it in log analytics work space.
- It takes 30 min to populate/collect the data

### Create a log analytics workspace
![](Pasted%20image%2020220714233647.png)
Region can be different from the resources region.
But when resources are in different region the data transfer charges apply. 

## Custom sources
Azure Monitor can collect log data from any REST client by using the [Data Collector API](https://docs.microsoft.com/en-us/azure/azure-monitor/logs/data-collector-api). You can create custom monitoring scenarios and extend monitoring to resources that don't expose telemetry through other sources.

## Application insights
**What is application insight**
Application Performance Management service for web developers.
You can use this tool to monitor your applications.
It can help developers detect anomalies in the application.
It can help diagnose issues.
It can also help understand how users use your application.
It also helps you improve performance and usability of your application.

**What gets monitored**
Request rates, the response times and failure rates — This is done at the page level.
Exception recorded by your application.
Page views and their load performance as reported from the user's browser.
User and session counts.
Performance counters of the underlying Windows or Linux Machines.
Diagnostic trace logs from your application.

# Service or Resource limits
We have already learnt on using services such as the Virtual Machine service. Now when using services it is important to understand that you have limits on how many resources you can create per service.

Let's understand the limit we have for Virtual Machines

1) First go to your subscriptions

![](https://img-c.udemycdn.com/redactor/raw/2020-04-13_05-19-43-c71bc0d1a009ea1125f70c1e15631181.jpg)

2) Then go to your subscription

![](https://img-c.udemycdn.com/redactor/raw/2020-04-13_05-20-37-78b532bf9084234512019f357c53c04b.jpg)

3) For the subscription , go to Usage + quotas

![](https://img-c.udemycdn.com/redactor/raw/2020-04-13_05-21-48-2bee91afc4b9115d6d567d06daf2373b.jpg)

4) In the right pane, then go to the "Select a provider" and choose Microsoft.Compute

![](https://img-c.udemycdn.com/redactor/raw/2020-04-13_05-24-50-978255d034e9b325d89ac6bd34f06799.jpg)

You will then see the quotas

![](https://img-c.udemycdn.com/redactor/raw/2020-04-13_05-26-48-143587bd96576bee23e73fbef71ddfc3.jpg)

So here , we have the Total Regional vCPUs quota for the West US region. I had gone ahead and create a virtual machine with 2 vCPU in the West US region, and that is why it shows that i have consumed 2 vCPUs out of 10 vCPUs in my quota.

Hence in the West US region, i can only spin up virtual machines that would together only have a maximum of 10 vCPUs.

![](https://img-c.udemycdn.com/redactor/raw/2020-04-13_05-28-53-90c2d6dabebe5ed910d1e60adc16f096.jpg)

If you want an increase in the quota limit, you can request an increase from Azure support

![](https://img-c.udemycdn.com/redactor/raw/2020-04-13_05-30-44-0c7c3a751469db79e56ab8b4b92bfd41.jpg)

For more information on service limits , one can visit the URL - [https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits)