---
# required metadata

title: Principles of operation | Microsoft Identity Manager
description:
keywords:
author: kgremban
manager: stevenpo
ms.date: 06/10/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid:

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

The solution for externalizing administrative accounts is composed of parallel forests. In this guide, there are two forests:

- *CORP*: A general-purpose corporate forest that includes one or more domains. Organizations may have multiple CORP forests, however this test lab guide for simplicity assumes a single forest with a single domain.  
- *PRIV*: A dedicated forest for privileged account management, created especially for this PAM scenario. This forest includes one domain. This domain will accommodate privileged groups and account which are shadowed from one or more CORP domains.

The Active Directory domain controller for the PRIV forest provides privileged user authentication and authorization. Furthermore, MIM enforces access through time-limited memberships of user accounts in security and foreign principal groups. Note that in the versions of the Windows Server referenced in this Test Lab Guide, foreign principal groups are not yet available; this will be provided in future updates to this Guide for the next version of Windows Server.

Microsoft Identity Manager adds new workflow activities and related resources for privileged access “just in time” elevation requests, which communicate directly to the Active Directory domain controller for the PRIV forest. It also provides new PowerShell cmdlets for elevation requests.

The MIM solution as configured for PAM includes the following components:  
- **MIM Service**: implements business logic for performing identity and access management operations, including privileged account management and elevation request handling.   
- **MIM Portal**: a SharePoint-based portal, hosted by SharePoint 2013, which provides an administrator management and configuration UI.
- **MIM Service Database**: stored in SQL Server 2012 or 2014, and holds identity data and meta-data required for MIM Service.
- **PAM Monitoring Service** and **PAM Component Service**: two services that manage the lifecycle of privileged accounts and assists the PRIV AD in group membership lifecycle.
- **PowerShell cmdlets**: for populating MIM Service and PRIV AD with users and groups that correspond to the users and groups in the CORP forest for PAM administrators, and for end users requesting just-in-time (JIT) use of privileges on an administrative account.
- **PAM REST API and sample portal**: for developers integrating MIM in the PAM scenario with custom clients for elevation, without needing to use PowerShell or SOAP. The use of the REST API is demonstrated with a sample web application.

Once installed and configured, each group created by the migration procedure in the PRIV forest is a shadow SIDHistory-based security group (or in a later update with Windows Server vNext, a foreign principal group) mirroring the SID group in the original CORP forest. Furthermore, when the MIM Service adds members to these groups in the PRIV forest, those memberships will be time limited.

As a result, when a user requests elevation using the PowerShell cmdlets, and their request is approved, the MIM Service will add their account in the PRIV forest to a group in the PRIV forest. When the user logs in with their privileged account, their Kerberos token will contain a Security Identifier (SID) identical to the SID of the group in the CORP forest. Since the CORP forest is configured to trust the PRIV forest, the elevated account being used to access a resource in the CORP forest appears, to a resource checking the Kerberos group memberships, be a member of that resource’s security groups. This is provided via Kerberos cross-forest authentication.

Furthermore, these memberships are time limited so that after a preconfigured interval of time, the user’s administrative account will no longer be part of the group in the PRIV forest. As a result, that account will no longer be usable for accessing additional resources.

For example, assume the CORP forest *CONTOSO* contains a group *CONTOSO\CorpAdmins* with a member *CONTOSO\Jen*. There is a sensitive resource, for instance a file share, whose access control list refers to that group. Because Jen is a member of that group, when Jen tries to access the file share, then she will have access.

After installing and configuring MIM, a new user is created in the PRIV forest: *PRIV.Jen*. This user account will not have any privileges by default. Also, a new group will be created in the PRIV forest: *PRIV\CONTOSO.CorpAdmins*. That group will not have any members by default. Furthermore, that group *PRIV\CONTOSO.CorpAdmins* has the same SID as *CONTOSO\CorpAdmins*.

At this point, *CONTOSO\Jen* can be removed (manually) from the *CONTOSO\CorpAdmins* group. When Jen requests access from MIM, and is authorized, then the MIM Service adds *PRIV.Jen* to *PRIV\CONTOSO.CorpAdmins*. Jen can then gain access to resources such as the file share using her new *PRIV.Jen* account. Later, that account’s membership in *PRIV\CONTOSO.CorpAdmins* is automatically terminated. If she is still needing access, Jen must re-request elevation.
