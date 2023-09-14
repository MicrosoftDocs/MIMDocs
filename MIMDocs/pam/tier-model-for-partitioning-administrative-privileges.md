---
# required metadata

title: PAM environment tier model | Microsoft Docs
description: Learn about the tier model that segregates your system based on vulnerability to risk.
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 09/14/2023
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---
# Tier model for partitioning administrative privileges

This article describes a security model intended to protect against elevation of privilege by segregating high-privilege activities from high-risk zones.

> [!IMPORTANT]
> The model in this article is intended only for isolated Active Directory environments using MIM PAM.  For hybrid environments, see instead the guidance in the [enterprise access model](/security/compass/privileged-access-access-model).

## Elevation of Privilege in Active Directory forests

Users, services, or applications accounts that are granted permanent administrative privileges to Windows Server Active Directory (AD) forests introduce a significant amount of risk to the organization’s mission and business. These accounts are often targeted by attackers because if they are compromised, the attacker has rights to connect to other servers or applications in the domain.

The tier model creates divisions between administrators based on what resources they manage. Admins with control over user workstations are separated from those that control applications, or manage enterprise identities.

## Restricting credential exposure with logon restrictions

Reducing credential theft risk for administrative accounts typically requires reshaping administrative practices to limit exposure to attackers. As a first step, organizations are advised to:

- Limit the number of hosts on which administrative credentials are exposed.
- Limit role privileges to the minimum required.
- Ensure administrative tasks are not performed on hosts used for standard user activities (for example, email and web browsing).

The next step is to implement logon restrictions and enable processes and practices to adhere to the tier model requirements. Ideally, credential exposure should also be reduced to the least privilege required for the role within each tier.

Logon restrictions should be enforced to ensure that highly privileged accounts do not have access to less secure resources. For example:

- Domain admins (tier 0) cannot log on to enterprise servers (tier 1) and standard user workstations (tier 2).
- Server administrators (tier 1) cannot log on to standard user workstations (tier 2).

>[!NOTE]
> Server administrators should not be in to the domain admin group. Personnel with responsibilities for managing both domain controllers and enterprise servers should be given separate accounts.

Logon restrictions can be enforced with:

- Group Policy Logon Rights Restrictions, including:
    - Deny access to this computer from the network
    - Deny logon as a batch job
    - Deny logon as a service
    - Deny logon locally
    - Deny logon through Remote Desktop settings  
- Authentication policies and silos, if using Windows Server 2012 or later
- Selective authentication, if the account is in a dedicated admin forest

## Next steps

- The next article, [Planning a bastion environment](planning-bastion-environment.md), describes how to add a dedicated administrative forest for Microsoft Identity Manager to establish the administrative accounts.
- Securing devices provide a dedicated operating system for sensitive tasks that is protected from Internet attacks and threat vectors.
