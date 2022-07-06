# Infra
## Availability set, fault domain and update domain
https://www.c-sharpcorner.com/article/availability-set-fault-domains-and-update-domains-in-azure-virtual-machie/

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
-  
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


# Azure Disks
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
