---
# required metadata

title: Understand the PAM components | Microsoft Docs
description: Privileged Access Management shares some components with MIM, and has a few of its own. Learn how these work together.
keywords:
author: henrymbuguakiarie
ms.author: henrymbuguakiarie
ms.date: 04/08/2025
ms.topic: article
ms.service: microsoft-identity-manager

ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---
# Understand the components of MIM PAM

Privileged Access Management keeps administrative access separate from day-to-day user accounts using a separate forest.

> [!NOTE]
> The PAM approach provided by MIM PAM is not recommended for new deployments in Internet-connected environments. MIM PAM is intended to be used in a custom architecture for isolated AD environments where Internet access is not available, where this configuration is required by regulation, or in high impact isolated environments like offline research laboratories and disconnected operational technology or supervisory control and data acquisition environments. MIM PAM is distinct from [Microsoft Entra Privileged Identity Management](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (PIM). Microsoft Entra PIM is a service that enables you to manage, control, and monitor access to resources in Microsoft Entra ID, Azure, and other Microsoft Online Services such as Microsoft 365 or Microsoft Intune. For guidance on on-premises Internet-connected environments and hybrid environments, see [securing privileged access](/security/compass/overview) for more information.

 This solution relies on parallel forests:

- *CORP*: Your general-purpose corporate forest that includes one or more domains. While you may have multiple CORP forests, the examples in these articles assume a single forest with a single domain for simplicity.  
- *PRIV*: A dedicated forest created especially for this PAM scenario. This forest includes one domain to accommodate privileged groups and accounts which are shadowed from one or more CORP domains.

The MIM solution as configured for PAM includes the following components:  

- **MIM Service**: implements business logic for performing identity and access management operations, including privileged account management and elevation request handling.
- **MIM Portal**: an optional SharePoint-based portal, hosted by SharePoint 2013 or later, which provides an administrator management and configuration UI.
- **MIM Service Database**: stored in SQL Server 2012 or later, and holds identity data and meta-data required for MIM Service.
- **PAM Monitoring Service** and if needed the **PAM Component Service**: two services that manage the lifecycle of privileged accounts and assists the PRIV AD in group membership lifecycle.
- **PowerShell cmdlets**: for populating MIM Service and PRIV AD with users and groups that correspond to the users and groups in the CORP forest for PAM administrators, and for end users requesting just-in-time (JIT) use of privileges on an administrative account.
- **PAM REST API**: for developers integrating MIM in the PAM scenario with custom clients for elevation, without needing to use PowerShell or SOAP. The use of the REST API is demonstrated with a sample web application.

Once installed and configured, each group created by the migration procedure in the PRIV forest is a foreign principal group mirroring the group in the original CORP forest. The foreign principal group provides users who are members of that group with the same SID in their Kerberos token as the SID of the group in the CORP forest. Furthermore, when the MIM Service adds members to these groups in the PRIV forest, those memberships will be time limited.

As a result, when a user requests elevation using the PowerShell cmdlets, and their request is approved, the MIM Service will add their account in the PRIV forest to a group in the PRIV forest. When the user logs in with their privileged account, their Kerberos token will contain a Security Identifier (SID) identical to the SID of the group in the CORP forest. Since the CORP forest is configured to trust the PRIV forest, the elevated account being used to access a resource in the CORP forest appears, to a resource checking the Kerberos group memberships, be a member of that resource’s security groups. This is provided via Kerberos cross-forest authentication.

Furthermore, these memberships are time limited so that after a preconfigured interval of time, the user’s administrative account will no longer be part of the group in the PRIV forest. As a result, that account will no longer be usable for accessing additional resources.
