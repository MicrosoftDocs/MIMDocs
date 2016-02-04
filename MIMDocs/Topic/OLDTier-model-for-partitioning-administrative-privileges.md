---
title: OLDTier model for partitioning administrative privileges
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5ec11328-34be-45c0-98d9-8ce786c1e003
---
# OLDTier model for partitioning administrative privileges
# Tier Model for Partitioning Administrative Privileges

## Introduction

Today’s threat environment has dissolved the effectiveness of a perimeter focused defense, though the perimeter is still a valid component of a larger strategy. The loss of this perimeter requires an organization to assume breach has already happened and to design defenses for computing and business resources accordingly. In order to enable organizations to manage at scale, this document describes a security model intended to protect against elevation of privilege, which provides a good user experience (while still adhering to best practices and security principles).

Partitioning administrative privileges into tiers simplifies the process of determining which users and groups are appropriate for inclusion in a *bastion environment* [link to "Planning a bastion environment](http://link1).

## Background: Elevation of Privilege in Active Directory forests

Users, services or applications accounts that are granted permanent full administrative privileges to Windows Server Active Directory forests introduce a significant amount of risk to the organization’s mission and business. These accounts are often targeted by attackers, for if they can be compromised, the attacker has typically then immediately has privilege to connect to arbitrary other servers or applications in the domain.

In some deployments, domains have been configured such that account operators and server operators are effective full administrators via the accounts, servers, and applications these roles can effectively control. In most cases these configurations have been made to support the need for an application to exercise administrative rights for either all clients and/or servers in the domain, or all user or computer accounts in the domain. Yet few applications require both of these kinds of rights, so granting directory administrator rights for both creates a state of over-permissioning that benefits an attacker or malicious insider.

## Tiered model for mitigating elevation of privilege

The following guidance provides a simple model for quickly classifying existing resources and setting up zones to limit account usage. This model adapts Biba and Bell-LaPadula hierarchical models to administrative control and is represented by three tiers of administrative privilege (plus a tier for end users who are not domain-wide administrators). To better partition and manage administrative access rights, accounts and applications are classified into one of four privilege tiers depending on their ability to impact the organization’s operations and mission:

- Tier 0: Forest Admins - Direct or indirect administrative control of the Active Directory forest, domains, or domain controllers

- Tier 1: Server Admins - Direct or indirect administrative control over a single or multiple servers

- Tier 2: Workstation Admins - Direct or indirect administrative control over multiple devices

- Tier 3: Users – Non-privileged users or administrative control over a single device

Specific business needs may require other tiers or additional segmentation, but this model can be used as a starting point.

![](./media/image1.png)

### Tiered Privilege Model Guidelines

The model is intended to prevent an escalation of privilege path for an attacker using stolen credentials and is defined by the following rules:

1. All resources to be managed (group, account, servers, workstation, Active Directory object, or application) will be classified in exactly one tier to prevent an escalation of privilege path for an attacker using credential theft techniques.

2. Personnel with responsibilities to logon to and manage resources at multiple tiers will have separate administrative accounts created for each required tier. Any account that currently logs into multiple categories will be split into multiple accounts that each fit within only one tier definition. These accounts will also be required to have different passwords.

3. Administrative accounts may not control higher tier resources through administrative access such as access control lists (ACLs), application agents, or control of service accounts. Accounts that control a higher tier may not log on to lower-tier computers because logging on to such a computer may expose and inadvertently grant control of the account credentials and privileges assigned to that account. Under some specific exceptions Remote Desktop connections, using RDP with restricted admin mode, could be used without exposing credentials.

4. Administrative accounts may control lower tier resources as required by their role, but only through management interfaces that are at the higher tier and that do not expose credentials—for example, domain admin accounts (tier 0) managing server admin Active Directory account objects (tier 1) through Active Directory management consoles on a domain controller (tier 0).

5. Each organizational unit (OU) that contains computer accounts may only contain computer accounts for that specific tier. If an OU that contains computer accounts for multiple tiers, the computer accounts for one tier will be moved to another domain, OU or separate sub-OUs will be created to house each respective tier.

### Tier 0 – Domain/Forest Administrator

Anyone with control over the groups, accounts, domain controllers, special Active Directory objects and applications in this tier has the ability to run arbitrary code anywhere in the domain or forest. The scope of Tier 0 is relative to a single active directory domain, though some accounts or other elements may have Tier 0 impact in multiple domains.

Tier 0 includes:

- Servers and workstations
  - Domain Controllers
  - Servers hosting a management application that controls an agent on a domain controller
  - Servers or workstations where Tier 0 accounts log in, including any server or workstation where a Tier 0 credential is exposed (such as servers/workstations that are running a service with the same account credentials that run a service on a Tier 0 server or are otherwise used to manage a Tier 0 server)

- Active Directory objects
  - The AdminSDHolder object in the system container of each domain
  - The system container in the domain naming context
  - The Domain Controllers OU
  - Any OU that contains Tier 0 objects (including parent OUs)
  - Any Group Policy linked to a Tier 0 OU

- Active Directory groups
  - Built-in and predefined AD groups, and members of those groups, including
    - Domain Admins
    - Enterprise Admins
    - Schema Admins
    - Builtin\\Administrators
    - Account Operators
    - Backup Operators

  - Groups which have been delegated Permissions equivalent to a tier 0 group (listed above), including
    - Ability to modify any or almost any object in Active Directory
    - Ability to reset passwords on any tier 0 account
    - Groups which have been granted modify or full control over any other Tier 0 object (user, group, computer, OU, group policy, or special objects)

- Accounts
  - The built-in administrator account
  - Accounts which are a member of any Tier 0 group
  - Accounts which have write or full control privileges over any Tier 0 object
  - Accounts which have been delegated permissions equivalent to a tier 0 group (including privileges listed above in Tier 0 groups)
  - Accounts which have administrative rights over applications in Tier 0

- Applications
  - Applications running as a service on domain controllers
  - Applications controlling an agent on domain controllers

- Hardware and devices
  - Hardware that Tier 0 systems are running on (note that this may depend on whether Shielded VMs are deployed for domain controllers)
  - Anyone with access to physical hardware of domain controllers, similar systems and backups
  - Anyone with administrative access to virtual machine hosts where Tier 0 virtualized computers are hosted
  - Devices where Tier 0 credentials are entered or stored (such as mobile devices used for remote access)

### Tier 1 – Server and Application Administrators

Anyone with control over the non-domain controller servers and administrative groups and accounts for them in the production Active Directory domain environment has the ability to run arbitrary code anywhere on those servers. The scope of Tier 1 is relative to a single active directory domain.

Tier 1 resources include all resources that meet the following criteria (and aren’t classified as Tier 0):

- Servers and workstations
  - Servers joined to the domain
  - Workstations where Tier 1 accounts log in, including any workstation where a Tier 1 credential is exposed (such as workstations that are running a service with the same account credentials that run a service on a Tier 1 server or are otherwise used to manage a Tier 1 server)

- Active Directory objects
  - Any OU that contains Tier 1 objects
  - Any Group Policy linked to a Tier 1 OU

- Active Directory groups
  - The Server Operators group (or a group that is a member of it)
  - Groups whose members are granted write or full control privileges over any Tier 1 object, or Groups which have been granted modify or full control over any Tier 1 object (user, group, computer, OU, or group policy objects)

- Accounts
  - Accounts which are a member of any Tier 1 group
  - Accounts which have write or full control privileges over any other Tier 1 object (frequently helpdesk accounts), including accounts been delegated Permissions equivalent to a tier 1 group (including privileges listed above in Tier 1 groups)
  - Accounts that are members of the local administrators group on one or more servers
  - Accounts which have administrative rights in Tier 1 applications

- Applications
  - Applications running as a service on tier 1 servers
  - Applications controlling an agent on tier 1 servers

- Hardware and devices
  - Hardware that Tier 1 systems are running on
  - Anyone with access to physical hardware for servers
  - Anyone with administrative access to virtual machine hosts where Tier 1 virtualized computers are hosted
  - Devices where Tier 1 credentials are entered or stored (such as mobile devices used for remote access)

### Tier 2 – Workstation and end-user administrators

Anyone with control over the workstations and standard users in the production Active Directory domain environment has the ability to run arbitrary code anywhere on those workstations or as those users. The scope of Tier 2 is relative to a single active directory domain.

Tier 2 resources include all resources that meet the following criteria (and aren’t classified as Tier 0 or Tier 1)

- Workstations
  - Workstations in the environment where any user may log on (not including administrative workstations used by Tier 1 or Tier 0 accounts)

- Active Directory objects
  - Any OU that contains Tier 2 objects
  - Any Group Policy linked to a Tier 2 OU

- Active Directory groups
  - Groups which have been granted modify or full control over any Tier 2 or Tier 3 object (user, group, computer, OU, or group policy objects)
  - Groups which are a member of the local administrators group on one or more workstations (e.g., help desk administrators and other PC and end-user support groups.)

- Accounts
  - Accounts which are a member of any Tier 2 group
  - Accounts which are a member of the local administrators group on one or more workstations

> [!Note] {This may include all end users if they are granted administrative rights on their workstations.}

- Accounts which have write or full control privileges over any other Tier 2 object
- Accounts with administrative rights for applications in Tier 2

- Applications
  - Applications running as a service on workstations
  - Applications controlling an agent on workstations

- Hardware and Devices
  - Hardware that Tier 2 systems are running on
  - Anyone with access to physical hardware for workstations
  - Anyone with administrative access to virtual machine hosts where Tier 2 virtualized computers are hosted
  - Devices where Tier 2 credentials are entered or stored (such as mobile devices used for remote access)

### Tier 3 – Standard Users

This tier describes standard users without administrative privileges over multiple computers in the domain.

## Restricting credential exposure with logon restrictions

Containing credential theft risk for administrative accounts typically requires reshaping administrative practices to limit exposure to attackers. As a first step, organizations are recommended to:

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

The next document, *planning a bastion environment* [link to adjoining TechNet article](http-//link-to-article), describes how to add a dedicated administrative forest for Microsoft Identity Manager to establish the administrative accounts.

  
