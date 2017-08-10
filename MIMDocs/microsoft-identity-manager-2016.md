---
# required metadata

title: Microsoft Identity Manager 2016 | Microsoft Docs
description: MIM includes the access management capabilities of FIM 2010 and helps you manage users, credentials, policies, and access within your organization.
keywords:
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Microsoft Identity Manager 2016
Microsoft Identity Manager (MIM) 2016 builds on the identity and access management capabilities of [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Like its predecessor, MIM helps you manage the users, credentials, policies, and access within your organization.  Additionally, MIM 2016 adds a hybrid experience, privileged access management capabilities, and support for new platforms.

This version of Microsoft Identity Manager provides new features such as Privileged Identity Management and support in Certificate Management for REST API access. In Certificate Management, there is now added support for multi-forest topologies, a Windows app for virtual smartcard and certificate lifecycle management, updated events and troubleshooting capabilities. Self-service scenarios now include Account Unlock and Azure MFA (multifactor authentication) gate for Password Reset.

## Hybrid experience
Microsoft Identity Manager 2016 works alongside Azure AD to give you control over your full environment. Hybrid reporting in Azure AD presents your cloud and on-premises data in one place. Also, the Self Service Password Reset portal supports Azure multi-factor authentication (MFA).

## Privileged Identity Management
Privileged Identity Management controls and manages administrative access by providing temporary, task-based access to sensitive resources. This means you can give users only as much permission as necessary, which lowers the chances of a cyber attacker gaining full administrative access. In addition, Privileged Identity Management extracts and isolates administrative accounts from existing Active Directory forests.

MIM supports an on-premises Privileged Identity Management solution for managing Active Directory. To get started, [Use Privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## Related topics

- Microsoft Identity Manager is still closely related to its predecessor, Forefront Identity Manager. If you still use FIM, or want to refer to additional documentation, take a look at the [FIM 2010 R2 Documentation Roadmap](https://technet.microsoft.com/library/jj133885.aspx).
- [Topology considerations for deploying MIM](topology-considerations.md)
- [Capacity planning guide](capacity-planning-guide.md)