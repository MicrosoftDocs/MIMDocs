---
# required metadata

title: Deployment Considerations for Privileged Access Management | Microsoft Identity Manager
description:
keywords:
author:
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: dd883415-d39f-4903-8b01-1c8b52e8d447

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Deployment Considerations for Privileged Access Management
The solution for externalizing administrative accounts is composed of parallel forests: the organizations' existing Active Directory forests and domains, and at least one new dedicated forest, for privileged access management (PAM). This forest, created specifically for the PAM scenario, has a domain that will accommodate privileged groups and accounts which are shadowed from one or more existing domains.

In addition to computer systems providing the Active Directory Domain Services (AD DS) for this privileged access management dedicated forest, the bastion environment also include computer systems hosting Microsoft Identity Manager (MIM) 2016, as well as SQL Server and its other dependencies. Also, privileged administrative workstations (PAW) computer systems may be joined to this domain.

The topics in this section describe:  
- [Planning a bastion environment](planning-bastion-environment.md)  
- [High availability and disaster recovery considerations for the bastion environment](high-availability-disaster-recovery-considerations-bastion-environment.md)  
- [Tier modeling for partitioning administrative privileges](tier-model-for-partitioning-administrative-privileges.md)  
- [Defining roles for privileged access management](defining-roles-for-pam.md)  
