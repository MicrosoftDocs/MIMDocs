---
title: High availability and disaster recovery considerations for the bastion environment
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03e521cd-cbf0-49f8-9797-dbc284c63018
author: Kgremban
---
# High availability and disaster recovery considerations for the bastion environment
This article describes considerations for High Availability (HA) and Disaster Recovery (DR) when deploying Active Directory Domain Services (AD DS) and Microsoft Identity Manager 2016 (MIM) for Privileged Access Management (PAM).

## High availability and disaster recovery scenarios
Enterprises have focused on high availability and disaster recovery for exiting workloads such as Windows Server, SQL Server and in particular, Active Directory.  A bastion environment for privileged access management provides additional isolation and controls for administration across server workloads in a Windows domain and forest environment.  The bastion environment can be a critical part of the organization's IT infrastructure, as users will be required to interact with its components in order to take on administrative roles.  So the availability of the bastion environment is also important. Security best practices require consideration of availability and disaster recovery across all IT workloads because natural disasters and determined attackers may target the bastion environment.  For more information on high availability in general, please see the white paper paper [Microsoft High Availability Overview](http://download.microsoft.com/download/3/B/5/3B51A025-7522-4686-AA16-8AE2E536034D/Microsoft%20High%20Availability%20Strategy%20White%20Paper.doc).

High availability and disaster recovery considerations include:

- What are the functions that could be impacted by an outage?
- Which of the functions are business critical and/or critical to IT operations?
- What are the risks that could lead to an outage in these systems?

The scope of these considerations impacts the total cost for deployment and operations, so organizations may prioritize certain functions higher than others, and also accept the risk of temporary outages for lower priority functions.  The bastion forest provides multiple functions, and some of them may not be business or IT critical.  The following table outlines one potential priority ranking an organization might use:

| **Bastion forest function** | **Relative priority during recovery** | **Mitigation if function unavailable** |
| --------------------------- | --------------------- | -------------- |
| Trust establishment         | Low | Wait until bastion environment is restored |
| User and group mitigation   | Low | Wait until bastion environment is restored |
| MIM administration          | Low | Wait until bastion environment is restored |
| Privileged role activation  | Medium | Dedicated smartcard-backed accounts to manually add users to administrative groups |
| Resource management         | High | Dedicated smartcard-backed accounts to manually add users to administrative groups |
| Monitoring of users and groups in existing forest | Low | Wait until bastion environment is restored | 

Now let's take a look at each one of these bastion forest functions in turn.

### Trust establishment
For users authenticating to the bastion environment to be able to administer resources in the existing forests, it is necessary to establish a forest trust between the domains of the existing forest and the bastion environment's forest. Additional configuration may be required, for instance to permit migration of users from existing domains on earlier versions of Windows Server.

For most organizations, domain structures do not change very frequently, and so trust establishment is often thought of as a one-time event, or when there is major scheduled change to Active Directory.
Trust establishment requires that the existing forest domain controllers be online, as well as the MIM and AD components of the bastion environment.  If there is an outage of one or more of these or the network during trust establishment, the administrator can retry once the outage has been addressed.  In case the existing forest domain controllers or bastion environment have been recovered following an outage, MIM also includes PowerShell cmdlets `Test-PAMTrust` and `Test-PAMDomainConfiguration` that can be used to verify that a trust is still in place.

### User and group migration
Once trust has been established, shadow groups can be created in the bastion environment, as well as user accounts for members of those groups and approvers.  This enables those users to activate privileged roles and re-gain effective group memberships.

For most organizations, group migration would occur only when establishing the bastion environment, when a new application or workload is added to the existing domains, or a change to group structure has been made to Active Directory.  User migration would occur as users join and leave the organization, and accounts are created and updated for them in the existing domains.  For many organizations, updating the existing domains from a HCM (human capital management) system is a scheduled process that may occur several times a day or several times a week.   

User and group migration requires that the existing forest domain controllers be online, as well as the MIM and AD components of the bastion environment.   If the existing forest domain controllers are unreachable, then no additional users and groups can be added to the bastion environment, but existing users and groups are unaffected. If an outage of one or more of the components or the network occurs during the migration, the administrator can retry once the outage has been addressed.

### MIM administration
Once users and groups have been migrated, then an administrator can further configure in MIM the role assignments linking users as candidates for activation into roles.  They can also configure the MIM policies for approval and Azure MFA.  

For most organizations, MIM administration in the bastion environment would occur subsequent to user or group migration, when users have been added or removed, or when there is a change in the business requirements or IT landscape. 

MIM administration requires that the MIM and AD components of the bastion environment be online.

### Privileged role activation
When a user wishes to activate a privileged role, they must authenticate to the bastion environment domain, and submit a request to MIM.  MIM includes SOAP and REST APIs, as well as user interfaces in PowerShell and in a web page.

Privileged role activation requires that the MIM and AD components of the bastion environment be online.  In addition, if MIM is configured to use [Azure MFA for activation](https://technet.microsoft.com/en-US/library/mt517876.aspx) of the selected role, then Internet access is required to contact the Azure MFA service.

### Resource Management
Once a user has been successfully activated into the role, the domain controller can generate a Kerberos ticket for them that is usable by domain controllers in the existing domains, that will recognize the user's new temporary group memberships.

Resource management requires that a domain controller for the resource domain be online, as well as a domain controller in the bastion environment.  Once a user is activated, issuing their Kerberos ticket does not require MIM or SQL to be online in the bastion environment.  (Note that with Windows Server 2012 R2 as the functional level for the bastion environment, MIM is required to be online, to terminate the temporary group membership.)

### Monitoring of users and groups in the exisitng forest
MIM also includes a PAM monitoring service which at intervals checks the users and groups in the existing domains, and updates the MIM database and AD accordingly.  This service is not needed to be online for role activation or during resource management. 

Monitoring requires that the existing forest domain controllers be online, as well as the MIM and AD components of the bastion environment.  

## Deployment options
[Getting Started with Privileged Access Management](https://technet.microsoft.com/en-us/library/mt345568.aspx) illustrates a basic topology suitable for learning the technology, which consists of two servers in the bastion environment, that is not intended for high availability.   This section describes how to expand upon that topology for providing high availability, for organizations with a single site as well as those with multiple existing sites.

### High availability for deployments within a single site
Most commonly, an organization will begin deploying a bastion environment by installing software on systems distinct from, but physically located, near the primary IT resource being managed.

### Networking
There are several recommendations for networking a bastion environment.

It is recommended that the network traffic between the computers in the bastion environment is isolated from the existing networks, such as by using a different physical or virtual network.  Depending on the risks to the bastion environment, it may be also necessary to have independent physical interconnects between the computers.  Certain failover cluster technologies have additional requirements on network interfaces (described below).

The computers hosting Active Directory Domain Services and those hosting the MIM Services in the bastion environment require bidirectional connectivity to resources in the existing forest for:
- users to be authenticated by the PRIV forest domain controllers 
- users to request activation
- users to have Kerberos tickets consumable by resources in existing forest 
- MIM to monitor the existing forest domains
- MIM to send email vail mail servers located in the existing forest.

### Minimal high availability topologies
An organization can select which functions in their bastion environment require high availability, with the following constraints:

- At a minimum, high availability for any function provided by the bastion environment requires at least two domain controllers.  
- High availability for activation requests requires at least two computers hosting the MIM Service and also requires high availability for the SQL Server.
- SQL Server high availability with failover clusters requires at least two servers providing SQL Server, and these cannot be the same as a domain controller.
- MIM Service should not be installed on the domain controller, in order to minimize the attack surface of each server.

The smallest high availability topology for all functions in a bastion environment comprises at least four servers, and shared storage. Two of the servers must be configured as domain controllers, providing Active Directory Domain Services. Two servers distinct from the domain controllers would be configured as a failover cluster providing SQL Server.  And two servers distinct from the domain controllers, would provide the MIM Service.

In addition, a typical deployment of the bastion environment would also include a privileged administration workstation for management of these servers, as well as a monitoring component

The following diagram illustrates one possible architecture:

![bastion1](/Image/bastion1.png)

Additional servers can be configured for each of these functions, in order to provide higher performance under load conditions, or for geographic redundancy as described below.

### Deployments supporting multiple sites
Selection of the appropriate deployment topology when an organization's resources are deployed across multiple sites will depend upon the goals and risk scenario for HA and disaster recovery, the hardware capability for hosting the bastion environment, and the administrative work model for each site.

One of the simplest approaches would be to host the bastion environment at a particular site.  Under normal conditions, users would connect to the MIM deployment in that site's bastion environment and request activation, and the activations would have effect across resources at each site.  In case the network link is broken or the site hosting the bastion environment is unavailable, offline credentials could be accessed at another site, in order to perform temporary administration until the network is reconnected.  This approach might be suitable for situations where local administration of a particular site, such as a branch office, is anticipated to be rare and limited to reconnecting that site to the remainder of an organization's network. 

![bastion2](/Image/bastion2.png)

For high availability and DR across sites, it is also possible to deploy the components of the bastion environment in each site, sharing a common PRIV directory and common SQL database.  In this topology, should the network link be broken, users at each site can continue to operate independently. 

![bastion3](/Image/bastion3.png)

One constraint on the above-illustrated deployment approach, however, is that SQL Server requires a cluster that spans both sites, which for some network and hardware choices may be complex to deploy. In that situation, consider as an alternative only replicating the Active Directory (PRIV forest) of the bastion environment.  In case there is a network break between sites, users in site B who have previously already activated their privileged roles would be able to continue to operate for administering resources in site B.

![bastion4](/Image/bastion4.png)

If each site represents a separate administrative boundary, then it is also possible to deploy multiple independent bastion environments.  While each bastion environment would have the same software, the domain names of each would be different, and there would be no commonality between the directories and databases of each bastion environment. A user who wishes to manage resources in a particular site would activate a user account in the bastion environment in that site. 

![bastion5](/Image/bastion5.png)

Finally, more complex deployments are possible, as multiple bastion environments may be configured independently to manage resources in a particular domain.

![bastion6](/Image/bastion6.png)

### Hosted bastion environment
Some organizations have also considered establishing the bastion environment separate from any of their existing sites. The bastion environment software can be hosted on a virtualization platform either within the organization's networks, or at an external hosting provider.  When evaluating this approach, keep in mind that:

- In order to protect against attacks originating from the existing domains, administration of the infrastructure on which the bastion environment is hosted must be isolated from the administrative accounts of the existing domain. 
- The bastion environment requires TCP/IP connectivity to the domain controllers in the existing domain.  A list of ports can be found at [How to configure a firewall for domains and trusts](https://support.microsoft.com/en-us/kb/179442).
- A virtualized deployment of Active Directory Domain Services requires specific features from the virtualization platform, as described in [Virtualized Domain Controller Deployment and Configuration](https://technet.microsoft.com/en-us/library/jj574223.aspx).
- A high availability deployment of SQL Server for MIM Service requires specialized storage configuration, described in the section "SQL Server database storage" below.  Not all hosting providers may currently offer Windows Server hosting with disk configurations suitable for SQL Server failover clusters. 

## Deployment preparation and recovery procedures
Preparing for a high availability or disaster recovery-ready deployment of the bastion environment requires consideration for how to install Windows Server Active Directory, SQL Server and its database on shared storage, and the MIM Service and its PAM components.

### Windows Server 
Windows Server contains a built-in feature for high availability, enabling multiple computers to work together as a failover cluster. The clustered servers are connected by physical cables and by software. If one or more of the cluster nodes fail, other nodes begin to provide service (a process known as failover).   More details can be found at the [Failover Clustering overview](https://technet.microsoft.com/en-us/library/hh831579.aspx).

#### Preparation
Scheduled maintenance is a normal part of server operation, and ensuring the operating system and applications in the bastion environment receive updates for security issues is critical to deployment.  As some of these updates may require a server restart, coordinating the times in which updates are applied across the servers is necessary to avoid extended outages.  One approach is to use [Cluster-Aware Updating](https://technet.microsoft.com/en-us/library/hh831694.aspx)  for the servers in a Windows Server failover cluster.

As the servers in the bastion environment will be joined to a domain, and dependent on the domain services, ensure that they are not inadvertently configured with a dependency on a particular domain controller for services such as DNS. 

### Bastion environment Active Directory
Windows Server Active Directory Domain Services natively includes support for high availability and disaster recovery.

#### Preparation 
A typical production deployment of privileged access management will include at least two domain controllers in the bastion environment.  Instructions for setting up the first domain controller in the bastion environment are included in the getting started guide, as [Prepare PRIV domain controller](https://technet.microsoft.com/en-us/library/mt345585.aspx).

The procedure for adding an additional domain controller can be found at [Install a Replica Windows Server 2012 Domain Controller in an Existing Domain (Level 200)](https://technet.microsoft.com/en-us/library/jj574134.aspx).  

**Note** if the domain controller is to be hosted on a virtualization platform such as Hyper-V, be sure to review the caveats in [Virtualized Domain Controller Deployment and Configuration](https://technet.microsoft.com/en-us/library/jj574223.aspx).

#### Recovery
After an outage, ensure that at least one domain controller is available in the bastion environment, before restarting other servers. 

Within a domain, Active Directory distributes the Flexible Single Master Operation (FSMO) roles across domain controllers, as described in [How Operations Masters Work](https://technet.microsoft.com/en-us/library/cc780487(v=ws.10).aspx).  If a domain controller has failed, it may be necessary to transfer one or more of the [Domain Controller Roles]() which that domain controller was assigned.

After determining that a domain controller will not be returned to production, be sure to check whether any roles were assigned to that domain controller, as described in [View the Current Operations Master Role Holders](https://technet.microsoft.com/en-us/library/cc816893(v=ws.10).aspx).  Instructions for reassigning those roles can be found at [Transfer the Schema Master], [Transfer the Domain Naming Master](https://technet.microsoft.com/en-us/library/cc816645(v=ws.10).aspx) and [Transfer the Domain-Level Operations Master Roles](https://technet.microsoft.com/en-us/library/cc816944(v=ws.10).aspx).  It is also recommended to check DNS settings of computers joined to the bastion environment, as well as the domain controllers in CORP domains which have a trust relationship to that domain controller, to ensure none are hard coded with a dependency on that domain controller computer's IP address.

### SQL Server database storage
A high availability deployment requires SQL Server failover clusters, and SQL Server failover cluster instances reply upon shared storage between all nodes for database and log storage. The shared storage can be in the form of Windows Server Failover Clustering cluster disks, disks on a Storage Area Network (SAN), or file shares on an SMB server.  Note that these must be dedicated to the bastion environment; sharing storage with other workloads outside of the bastion environment is not recommended as it could jeopardize the integrity of the bastion environment. 

#### Preparation and Recovery
The preparation and recovery procedures will depend upon the type of storage selected.

### SQL Server
MIM Service requires a SQL Server deployment in the bastion environment.  There are multiple options for how SQL Server can be deployed.  For High Availability, SQL can be deployed using a failover cluster instance (FCI). When a SQL Server instance is configured to be an FCI (instead of a standalone instance), the high availability of that SQL Server instance is protected by the presence of redundant nodes in the FCI. In case of a failure (hardware failures, operating system failures, application or service failures), or a planned upgrade, the resource group ownership is moved to another Windows Server Failover Cluster node.

If only support for the disaster recovery is needed but not high availability, then log shipping, transaction replication, snapshot replication or database mirroring can be used instead of failover clustering.   

#### Preparation
When you install the SQL Server in the bastion environment, it must be independent from any SQL Server already present in the CORP forests outside of the bastion environment.  Furthermore, it is recommended that the SQL Server be deployed on a dedicated server, distinct from that of the domain controller. 
More information is documented in the SQL Server guide to [AlwaysOn Failover Cluster Instances](https://msdn.microsoft.com/en-us/library/ms189134(v=sql.120).aspx). 

#### Recovery
If SQL Server was configured for disaster recovery using log shipping, then action must be taken to update SQL Server during recovery.  Furthermore, restarting each MIM Service instance is required.

If SQL Server has failed or connectivity between SQL Server and MIM Service has been lost, then after SQL Server has been restored, it is recommended to restart each MIM Service.  This will ensure that MIM Service re-establishes its connection to SQL Server. 

### MIM Service
The MIM Service is required to process activation requests.  In order that a computer hosting MIM Service can be taken down for maintenance while activation requests are still being received, multiple MIM Service computers can be deployed.  Note that MIM Service is not involved in Kerberos operations once a user has been added to a group.  

#### Preparation
It is recommended to deploy the MIM Service on multiple servers joined to the PRIV domain. 
For high availability, Windows Server documents the [Failover Clustering Hardware Requirements and Storage Options](https://technet.microsoft.com/en-us/library/jj612869.aspx) and the process for [Creating a Windows Server 2012 Failover Cluster](http://blogs.msdn.com/b/clustering/archive/2012/05/01/10299698.aspx). 

Instructions on setting up the first server for MIM Service are included in the getting started guide, in [Prepare a PAM server](https://technet.microsoft.com/en-us/library/mt345587.aspx) and [Install MIM components on PAM server and workstation](https://technet.microsoft.com/en-us/library/mt345588.aspx). If SQL Server has already been deployed in the bastion environment, then it is not necessary to install SQL Server when performing step 3 to prepare a PAM server.  When performing step 4 to install MIM components on the PAM server, on the first installation of MIM Service, specify to create a new database, and on subsequent installations of MIM Service, specify to use an existing database.

For production deployment, across multiple servers, you can use Network Load Balancing (NLB) to distribute the processing load.  You should also have a single alias (for instance, A or CNAME records) so that one common name is exposed to the user. 

**Important** 
If you use a load-balancing technology other than the NLB feature in Windows Server 2012 R2, make sure your solution will redirect one session to the same server and not to a random server.

In a multi-server MIM deployment, each MIM Service has an external host name, a service name, and a service partition name.  The default value of the service name is the computer's name, and the default value of the external hostname and service partition name are configured during MIM Service installation on the screen that asks for the MIM Service Server address. These three names are stored in file %ProgramFiles%\Microsoft Forefront Identity Manager\Service\Microsoft.ResourceManagementService.exe.config file as attributes `externalHostName`, `serviceName` and `servicePartitionName` of the `resourceManagementService` configuration node.  

When a MIM Service receives a request, the service partition name is stored as an attribute on that request.   Subsequently, only other MIM Service installations that have the same service partition name are permitted to interact with that request.  As a result, if the PAM scenario includes manual approvals or other long-lived request processing, ensure that each MIM Service has the same `servicePartitionName` attribute in that configuration file. 

#### Recovery 
After an outage, ensure that at least one Active Directory domain controller and SQL Server are available in the bastion environment prior to restarting MIM Service.  

A workflow instance can only be completed by a MIM Service server that has the same service partition name and service name as the MIM Service server which started it.  If a particular computer fails while hosting a MIM Service that was processing requests, and that computer will not be returned to service, then it will be necessary to install MIM Service on a new computer. On the new MIM Service after installation, edit the *resourcemanagementservice.exe.config* file and set the `serviceName` and `servicePartitionName` attributes of the new MIM deployment to be the same as the host name and service partition name of the computer which failed. 

### MIM PAM components
The MIM Service and Portal installer also incorporates additional PAM components, including PowerShell modules and two services. 

#### Preparation
The Privileged Access Management components should be installed on each computer in the bastion environment where MIM Service is being installed.  They cannot be added subsequently.

#### Recovery
After recovery from an outage, ensure that the MIM Service is running on at least one server.  Then ensure that the MIM PAM monitoring service is also running on that server, using 

`net start "PAM Monitoring service"`

If the bastion environment forest functional level is Windows Server 2012 R2, ensure that the MIM PAM component service is also running on that server, using the command

`net start "PAM Component service"`