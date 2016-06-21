---
# required metadata

title: Define roles for Privileged Access Management | Microsoft Identity Manager
description:
keywords:
author: kgremban
manager: femila
ms.date: 06/14/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 1a368e8e-68e1-4f40-a279-916e605581bc

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Define roles for Privileged Access Management

With Privileged Access Management, you can assign users to privileged roles that they can activate as needed for just-in-time access. These roles are definied manually and established in the bastion environment. This article walks you through the process of deciding which roles to manage through PAM, and how to define them with appropriate permissions and restrictions.

A straightforward approach to defining roles for privileged access management is to compile all the information in a spreadsheet. List the roles in the roles, and use the columns to identify governance requirements and permissions.

The governance requirements will vary depending on existing identity and access policies or compliance requirements. The parameters to identify for each role might include the owner of the role, the candidate users who can be in that role, and what authentication, approval, or notification controls should be associated with the use of the role.

The role permissions depend on the applications being managed. This article uses Active Directory as an example application, dividing the permissions into two categories:

- Those needed to manage the Active Directory service itself (e.g., configure the replication topology)

- Those needed to manage the data held in Active Directory (e.g., create user and groups)

## Identify roles

Start by identifying all the roles that you might want to manage with PAM. On the spreadsheet, each potential role will have its own row.

To find the appropriate roles, consider each application in scope for management:

- Is the application in tier 0, tier 1 or tier 2?  
- What are the privileges that impact the confidentiality, integrity or availability of the application?  
- Does the application have dependencies on other components of the system, such as databases, network or security infrastructure, or the virtualization or hosting platform?

Determine how to group those app considerations. You want roles that have clear boundaries, and give only enough permissions to complete common administrative tasks within the app.

You always want to design roles for the least privilege assignment. This can be based on the current (or planned) organizational responsibilities for users, and would include the privilege required by the user's duties. It could also include the privileges that simplify operations, without creating risk.

Other considerations in scoping the permissions to include a role are:

- How many individuals are working in a particular role? If there are not at least 2, then it may be too narrowly defined to be useful, or you've defined a particular person's duties.

- How many roles does a person take on? Would users be able to select the right role for their task?

- Would the user population and how they interact with the applications be compatible with privileged access management?

- Is it possible to separate administration and audit, so that a user in an administrative role cannot erase the audit records of their actions?

## Establish role governance requirements

As you identify candidate roles, start to fill out the spreadsheet. Create columns for the requirements which are relevant to your organization. Some requirements to consider include:

- Who is the role owner that would be responsible for the further definition of role, choosing permissions, and maintaining the governance settings for the role?

- Who are the role holders (users) who will perform the role duties or tasks?

- What access method (discussed in the next section) would be appropriate for the role holders?

- Is manual approval by a role owner required when a user activates their role?

- Is notification required when a user activates their role?

- Should use of this role generate an alert or notification in a SIEM system, for tracking purposes?

- Is it necessary to restrict users activating the role to only be able to log onto computers where access is required to perform the role duties, and where there is sufficient verification of the host that it will be able to secure the privileges/credentials from misuse?

- Is it required to provide a dedicated admin workstation to the role holders?

- Which application permissions (see example list for AD below) are associated with this role?

## Select an access method

There may be multiple roles in a privileged access management system with the same permissions assigned to them, if different communities of users have distinct access governance requirements. For example, an organization may apply different policies for their full-time employees than for outsourced IT employees of another organization.

In some cases, a user may be permanently assigned to a role, and so they do not need to request or activate a role assignment. Examples of permanent assignment scenarios include:

- A managed service account in existing forest

- A user account in the existing forest, with a credential managed outside of PAM (for example, a "break glass" account, in which a role such as "Domain / DC maintenance" needed to fix trust and DC health problems is permanently assigned to the account, with a physically secured password)

- A user account in the administrative forest who authenticates with a password (for example, a user who needs permanent 24x7 administrative permissions and logs on from a device which cannot support strong authentication)

- A user account in the administrative forest, with a smartcard or virtual smartcard (for example, an account with an offline smartcard, needed for rare maintenance tasks)

For organizations concerned about the potential for credential theft or misuse, the [Using Azure MFA for activation](use-azure-mfa-for-activation.md) guide includes instructions for how to configure MIM to require an additional out of band check at the time of role activation.

## Delegate Active Directory permissions

Windows Server automatically creates default groups such as "Domain Admins" when new domains are created. These groups simplify getting started and may be suitable for smaller organizations. However, larger organizations, or those requiring more isolation of administrative privileges, should empty groups like Domain Admins and replace them with groups that provide fine-grained permissions.

One limitation of the Domain Admins group is that it cannot have members from an external domain. Another limitation is that it grants permissions for three separate functions:  
- Managing the Active Directory service itself  
- Managing the data held in Active Directory  
- Enabling remote logon onto domain joined computers.

In place of default groups like Domain Admins, create new security groups that provide only the necessary permissions, and use MIM to dynamically provide administrator accounts with those group memberships.

### Service management permissions

The following table gives examples of permissions which would be relevant to include in roles for managing AD.

| Role | Description |
| ---- | ---- |
| Domain/DC Maintenance | Membership in the Domain\Administrators group that allows for troubleshooting and altering the domain controller operating system, promoting a new domain controller into an existing domain in the forest and AD role delegation.
|Manage Virtual DCs | Manage domain controller (DC) virtual machines (VMs) using the virtualization management software. This privilege may be granted via full control of all virtual machines in the management tool or Role-based access control (RBAC) functionality. |
| Extend Schema | Manage the schema, including adding new object definitions, altering permissions to schema objects, and altering schema default permissions for object types |
| Backup Active Directory Database | Take a backup copy of the Active Directory Database in its entirety, including all secrets entrusted to the DC and the Domain. |
| Manage Trusts and Functional Levels | Create and delete trusts with external domains and forests. |
| Manage Sites, Subnets, and Replication | Manage the Active Directory replication topology objects including modifying sites, subnets, and site link objects and initiating replication operations |
| Manage GPOs | Create, delete, and modify Group Policy Objects throughout the domain |
| Manage Zones | Create, delete, and modify DNS Zones and objects in the Active Directory |
| Modify Tier 0 OUs | Modify Tier 0 OUs and contained objects in the Active Directory |

### data management permissions

The following table gives examples of permissions which would be relevant to include in roles for managing or using the data held in AD.

| Role | Description |
| ---- | ---- |
| Modify Tier 1 Admin OU                 | Modify OUs containing Tier 1 Admin objects in the Active Directory |
| Modify Tier 2 Admin OU                 | Modify OUs containing Tier 2 Admin objects in the Active Directory |
| Account Management: Create/Delete/Move | Modify standard user accounts                                      |
| Account Management: Reset Unlock       | Reset passwords and unlock accounts                                  |
| Security Group: Create Modify          | Create and modify security groups in Active Directory              |
| Security Group: Delete                 | Delete security groups in Active Directory                         |
| GPO Management                         | Manage all GPOs in domain/forest that don't affect Tier 0 servers             |
| Join PC/Local Admin                    | Local administrative rights to all workstations                               |
| Join Srv/Local Admin                   | Local administrative rights to all servers                                    |

## Example role definitions

The choice of role definitions will depend upon the tier of servers being managed by the privileged accounts. It will also depend upon the choice of applications being managed, since applications like Exchange or third party enterprise products such as SAP will often bring their own additional role definitions for delegated administration.

The following sections give examples for typical enterprise scenarios.

### Tier 0 - Administrative forest

Roles suitable for accounts in the bastion environment might include:

- Emergency access to the administrative forest
- "Red Card" admins: users who are administrators of the administrative forest
- Users who are administrators of the production forest
- Users who are delegated limited administrative rights to applications in the production forest

### Tier 0 - Enterprise production forest

Roles suitable for managing the tier 0 production forest accounts and resources might include:

- Emergency Access to the production forest
- Group Policy admins
- DNS admins
- PKI admins
- AD Topology and Replication admins
- Virtualization admins for Tier 0 servers
- Storage admins
- Anti-Malware admins for Tier 0 servers
- SCCM admins for Tier 0 SCCM
- SCOM Admins for Tier 0 SCOM
- Backup admins for Tier 0
- Users of out-of-band and baseboard management controllers (for KVM or lights-out management) connected to Tier 0 hosts

### Tier 1

Roles for management and backup of servers in Tier 1 might include:

- Server maintenance
- Virtualization admins for Tier 1 servers
- Security Scanner Account
- Anti-Malware admins for Tier 1 servers
- SCCM admins for Tier 1 SCCM
- SCOM admins for Tier 1 SCOM
- Backup admins for Tier 1 servers
- Users of out-of-band and baseboard management controllers (for KVM or lights-out management) to Tier 1 hosts

Also, roles for managing enterprise applications in Tier 1 might include:

- DHCP administrators
- Exchange administrators
- Skype for Business administrators
- SharePoint farm administrators
- Administrators of a cloud service, e.g., a company Web Site or public DNS
- Administrators of HCM, Financial or Legal systems

### Tier 2

Roles for non-administrative user and computer management might include:

- Account admins
- Helpdesk
- Security group admins
- Workstation deskside support
