---
# required metadata

title: Tier model for partitioning administrative privileges | Microsoft Identity Manager
description:
keywords:
author: kgremban
manager: stevenpo
ms.date: 06/10/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
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
## Introduction

Today’s threat environment has dissolved the effectiveness of a perimeter focused defense, though the perimeter is still a valid component of a larger strategy. The loss of this perimeter requires an organization to assume breach has already happened and to design defenses for computing and business resources accordingly. In order to enable organizations to manage at scale, this document describes a security model intended to protect against elevation of privilege, which provides a good user experience (while still adhering to best practices and security principles).

Partitioning administrative privileges into tiers simplifies the process of determining which users and groups are appropriate for inclusion in a [bastion environment](planning-bastion-environment.md).

## Background: Elevation of Privilege in Active Directory forests

Users, services, or applications accounts that are granted permanent full administrative privileges to Windows Server Active Directory forests introduce a significant amount of risk to the organization’s mission and business. These accounts are often targeted by attackers, for if they can be compromised, the attacker then has privilege to connect to other servers or applications in the domain.

In some deployments, domains have been configured such that account operators and server operators are effective full administrators via the accounts, servers, and applications these roles can effectively control. In most cases these configurations have been made to support the need for an application to exercise administrative rights for either all clients and/or servers in the domain, or all user or computer accounts in the domain. Yet few applications require both of these kinds of rights, so granting directory administrator rights for both creates a state of over-permissioning that benefits an attacker or malicious insider.

## Tier model for mitigating elevation of privilege

The tier model creates divisions between administrators based on what resources they manage. Admins with control over user workstations are separated from those that control applications, or manage enterprise identities. Get more details about this model in the [Securing privileged access reference material](http://aka.ms/tiermodel).

## Restricting credential exposure with logon restrictions

Containing credential theft risk for administrative accounts typically requires reshaping administrative practices to limit exposure to attackers. As a first step, organizations are advised to:

- Limit the number of hosts on which administrative credentials are exposed.
- Limit role privileges to the minimum required.
- Ensure administrative tasks are not performed on hosts used for standard user activities (for example, email and web browsing).

The next step is to implement logon restrictions and enable processes and practices to adhere to the tier model requirements. Ideally, credential exposure should also be reduced to the least privilege required for the role within each tier:

Logon restrictions should be enforced to ensure that

- Domain admins (tier 0) cannot log on to enterprise servers (tier 1) and standard user workstations (tier 2).
- Server administrators (tier 1) cannot log on to standard user workstations (tier 2).

>[Note] {Server administrators should not be in to the domain admin group. Personnel with responsibilities for managing both domain controllers and enterprise servers should be given separate accounts.}

Logon restrictions can be enforced with:

- Group Policy Logon Rights Restrictions, including the Deny access to this computer from the network, Deny logon as a batch job, Deny logon as a service, Deny logon locally, Deny logon through Remote Desktop settings
- Authentication policies and silos, if using Windows Server 2012 or later
- Selective authentication, if the account is in a dedicated admin forest

The next document, [Planning a bastion environment](planning-bastion-environment.md), describes how to add a dedicated administrative forest for Microsoft Identity Manager to establish the administrative accounts.
