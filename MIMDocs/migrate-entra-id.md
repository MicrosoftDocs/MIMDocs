---

title: Migrating to Microsoft Entra ID from Microsoft Identity Manager
description: This document describes migration options and approaches for moving identity and access management scenarios from Microsoft Identity Manager to Microsoft Entra.
services: active-directory
documentationcenter: ''
keywords: MIM
author: henrymbuguakiarie
ms.author: henrymbuguakiarie
reviewer: markwahl-msft
ms.date: 04/08/2025
ms.topic: article
ms.service: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity

---

# Migrating Identity and Access Management scenarios to Microsoft Entra from Microsoft Identity Manager
Microsoft Identity Manager is Microsoft’s on-premises-hosted identity and access management product. It's based on technology introduced in 2003, continuously improved through today, and [supported along with Microsoft Entra cloud services](microsoft-identity-manager-2016.md?#support-update-for-microsoft-entra-id-p1-or-p2-customers). MIM has been a core part of many identity and access management strategies, augmenting Microsoft Entra ID's cloud-hosted services and other on-premises agents.

>[!IMPORTANT]
>We are seeking feedback from our customers on how they are planning their migration from Microsoft Identity Manager (MIM) before its end of life in January 2029.
>
>Please take the time to fill out a small survey here:  https://aka.ms/MIMMigrationFeedback 

Many customers have expressed interest in moving the center of their identity and access management scenarios entirely to the cloud. Some customers will no longer have an on-premises environment, while others integrate the cloud-hosted identity and access management with their remaining on-premises applications, directories and databases. This document provides guidance on migration options and approaches for moving Identity and Access Management (IAM) scenarios from Microsoft Identity Manager to Microsoft Entra cloud-hosted services, and will be updated as new scenarios become available to migrate. Similar guidance is available for migration of other on-premises identity management technologies, including [migrating from ADFS](/entra/identity/enterprise-apps/migrate-ad-fs-application-howto).

## Migration overview

MIM implemented the best practices of identity and access management at the time of its design. Since then, the identity and access management landscape has evolved with new applications and new business priorities, and so the approaches recommended for addressing IAM use cases will in many cases be different today than those previously recommended with MIM.

In addition, organizations should plan a staged approach for scenario migration. For example, an organization may prioritize migrating an end-user self-service password reset scenario as one step, and then once that is complete, moving a provisioning scenario. The order in which an organization chooses to move their scenarios will depend upon their overall IT priorities and the impact on other stakeholders, such as end users needing a training update, or application owners.

| IAM scenario in MIM | Link for more information on IAM scenario in Microsoft Entra |
| :--- | :--- |
| Provisioning from SAP HR sources | [bring identities from SAP HR into Microsoft Entra ID](/entra/id-governance/sap#bring-identities-from-hr-into-microsoft-entra-id) |
| Provisioning from Workday and other cloud HR sources | [provisioning from cloud HR systems into Microsoft Entra ID with join/leave lifecycle workflows](#provisioning-from-cloud-hr-systems-to-active-directory-or-microsoft-entra-id-with-joinleave-workflows) |
| Provisioning from other on-premises HR sources| [provisioning from on-premises HR systems with join/leave lifecycle workflows](#provisioning-users-from-on-premises-hr-systems-to-microsoft-entra-id-with-joinleave-workflows)|
| Provisioning to non-AD-based on-premises applications | [provisioning users from Microsoft Entra ID to on-premises apps](#provisioning-users-from-microsoft-entra-id-to-on-premises-apps) |
| Global address list (GAL) management for distributed organizations | [synchronization of users from one Microsoft Entra ID tenant to another](/entra/identity/multi-tenant-organizations/cross-tenant-synchronization-overview)|
| AD security groups| [govern on-premises Active Directory based apps (Kerberos) using Microsoft Entra ID Governance](/entra/identity/hybrid/cloud-sync/govern-on-premises-groups) |
| Dynamic groups | [rule-based Microsoft Entra ID security group and Microsoft 365 group memberships](#dynamic-groups)|
| Self-service group management | [self-service Microsoft Entra ID security group, Microsoft 365 groups and Teams creation and membership management](#self-service-group-management)|
| Self-service password management | [self-service password reset with writeback to AD](#self-service-password-reset) |
| Strong credential management | [passwordless authentication for Microsoft Entra ID](/entra/identity/authentication/concept-authentication-passwordless) |
| Historical audit and reporting |[archive logs for reporting on Microsoft Entra ID and Microsoft Entra ID Governance activities with Azure Monitor](/entra/id-governance/entitlement-management-logs-and-reporting) |
| Privileged access management | [securing privileged access for hybrid and cloud deployments in Microsoft Entra ID](/entra/identity/role-based-access-control/security-planning)|
| Business role-based access management |[govern access by migrating an organizational role model to Microsoft Entra ID Governance](/entra/id-governance/identity-governance-organizational-roles) |
| Attestation | [access reviews for group memberships, application assignments, access packages and roles](/entra/id-governance/access-reviews-overview)|


## User provisioning
User provisioning is at the very heart of what MIM does. Whether it's AD or other HR sources, importing users, aggregating them in the metaverse and then provisioning them to different repositories is one of its core functions. The diagram below illustrates a classic provisioning / synchronization scenario. 

 :::image type="content" source="media/migrate-entra-id/on-prem-3.png" alt-text="Conceptual drawing of on-premises provisioning with MIM." lightbox="media/migrate-entra-id/on-prem-3.png":::

Now many of these user provisioning scenarios are available using Microsoft Entra ID and related offerings, that allow you to migrate those scenarios off of MIM to manage accounts in those applications from the cloud. 

The following sections describe the various provisioning scenarios.


### Provisioning from cloud HR systems to Active Directory or Microsoft Entra ID with join/leave workflows

 :::image type="content" source="media/migrate-entra-id/scenario-1-a.png" alt-text="Conceptual drawing of cloud provisioning to Microsoft Entra ID and AD." lightbox="media/migrate-entra-id/scenario-1-a.png":::

Whether you want to provision directly from the cloud in to Active Directory or Microsoft Entra ID, this can be accomplished using built-in integrations to Microsoft Entra ID. The following tutorials provide guidance on provisioning directly from your HR source in to AD or Microsoft Entra ID.

- [Tutorial: Configure Workday for automatic user provisioning](/entra/identity/saas-apps/workday-inbound-tutorial) 
- [Tutorial: Configure Workday to Microsoft Entra user provisioning](/entra/identity/saas-apps/workday-inbound-cloud-only-tutorial)

Many of the cloud HR scenarios also involve using automated workflows. Some of these workflow activities that were developed using the Workflow Activity Library for MIM can be migrated to Microsoft ID Governance Lifecycle workflows. Many of these real world scenarios can now be created and managed directly from the cloud. For more information, see the following documentation.

 - [What are lifecycle workflows?](/entra/id-governance/what-are-lifecycle-workflows)
 - [Automate employee on-boarding](/entra/id-governance/tutorial-onboard-custom-workflow-portal)
 - [Automate employee off-boarding](/entra/id-governance/tutorial-offboard-custom-workflow-portal)

### Provisioning users from on-premises HR systems to Microsoft Entra ID with join/leave workflows

Customers who use SAP Human Capital Management (HCM) and have SAP SuccessFactors can bring identities into Microsoft Entra ID by using SAP Integration Suite to synchronize lists of workers between SAP HCM and SAP SuccessFactors. From there, you can bring identities directly into Microsoft Entra ID or provision them into Active Directory Domain Services.

![Diagram of SAP HR integrations.](/entra/id-governance/media/sap/sap-hr-no-MIM.png)


Using API-driven inbound provisioning, it's now possible to provision users directly to Microsoft Entra ID from your on-premises HR system. If you're currently using a MIM to import users from an HR system and then provision them to Microsoft Entra ID, you can now use build a custom API-driven inbound provisioning connector to accomplish this. The advantage of using the API-driven provisioning connector to achieve this over MIM, is that the API-driven provisioning connector has a lot less overhead and a lot smaller footprint on-premises, when compared with MIM. Also, with the API-driven provisioning connector, it can be managed from the cloud. See the following for more information on API-driven provisioning.

- [API-driven inbound provisioning concepts](/entra/identity/app-provisioning/inbound-provisioning-api-concepts)
- [Enable system integrators to build more connectors to systems of record](/entra/identity/app-provisioning/inbound-provisioning-api-concepts#scenario-3-enable-system-integrators-to-build-more-connectors-to-systems-of-record)
- [Configure API-driven inbound provisioning app](/entra/identity/app-provisioning/inbound-provisioning-api-configure-app)

 :::image type="content" source="media/migrate-entra-id/scenario-1-b.png" alt-text="Conceptual drawing of API-driven provisioning to Microsoft Entra ID." lightbox="media/migrate-entra-id/scenario-1-b.png":::

These can also leverage lifecycle workflows as well.

 - [What are lifecycle workflows?](/entra/id-governance/what-are-lifecycle-workflows)
 - [Automate employee on-boarding](/entra/id-governance/tutorial-onboard-custom-workflow-portal)
 - [Automate employee off-boarding](/entra/id-governance/tutorial-offboard-custom-workflow-portal)

### Provisioning users from Microsoft Entra ID to on-premises apps
 :::image type="content" source="media/migrate-entra-id/scenario-1-c.png" alt-text="Conceptual drawing of provisioning to on-premises apps." lightbox="media/migrate-entra-id/scenario-1-c.png":::

 If you're using MIM to provision users to applications such as SAP ECC, to applications that have a SOAP or REST API, or to applications with an underlying SQL database or non-AD LDAP directory, you can now use on-premises application provisioning via the ECMA Connector Host to accomplish the same tasks. The ECMA Connector Host is part of a light-weight agent and allows you to reduce your MIM footprint. If you have custom connectors in your MIM environment, you can migrate their configuration to the agent. For more information, see the documentation below.

- [On-premises provisioning application architecture](/entra/identity/app-provisioning/on-premises-application-provisioning-architecture)
- [Provisioning users to SCIM-enabled apps](/entra/identity/app-provisioning/on-premises-scim-provisioning)
- [Provisioning users into SQL based applications](/entra/identity/app-provisioning/on-premises-sql-connector-configure)
- [Provisioning users into LDAP directories](/entra/identity/app-provisioning/on-premises-ldap-connector-configure)
- [Provisioning users into an LDAP directory for Linux authentication](/entra/identity/app-provisioning/on-premises-ldap-connector-linux)
- [Provisioning users into applications using PowerShell](/entra/identity/app-provisioning/on-premises-powershell-connector)
- [Provisioning with the web services connector](/entra/identity/app-provisioning/on-premises-web-services-connector)
- [Provisioning with the custom connectors](/entra/identity/app-provisioning/on-premises-custom-connector)

### Provision users to cloud SaaS apps
:::image type="content" source="media/migrate-entra-id/scenario-1-d.png" alt-text="Conceptual drawing of provisioning to Saas apps." lightbox="media/migrate-entra-id/scenario-1-d.png":::

Integrating with SaaS applications is needed in the world of cloud computing. Many of the provisioning scenarios that MIM was performing to SaaS apps, can now be done directly from Microsoft Entra ID. When configured, Microsoft Entra ID automatically provisions and deprovisions users to SaaS applications using the Microsoft Entra provisioning service. For a complete list of SaaS app tutorials, see the link below.

- [Tutorials for integrating with SaaS apps](/entra/identity/saas-apps/tutorial-list)

### Provision users and groups to new custom apps

If your organization is building new applications, and requires receiving user or group information or signals when users are updated or deleted, then we recommend the application either use Microsoft Graph to query Microsoft Entra ID, or use SCIM to be automatically provisioned.

- [Using the Microsoft Graph API](/graph/use-the-api)
- [Develop and plan provisioning for a SCIM endpoint in Microsoft Entra ID](/entra/identity/app-provisioning/use-scim-to-provision-users-and-groups)
- [Provisioning users to SCIM-enabled applications that are on-premises](/entra/identity/app-provisioning/on-premises-scim-provisioning)

## Group management scenarios

Historically organizations used MIM for managing groups in AD, including AD security groups and Exchange DLs, which were then synchronized through Microsoft Entra Connect to Microsoft Entra ID and Exchange Online. Organizations can now manage security groups in Microsoft Entra ID and Exchange Online, without requiring groups to be created in on-premises Active Directory.

### Dynamic groups
If you're using MIM for dynamic group membership, these groups can be migrated to be Microsoft Entra ID Dynamic groups. With attribute-based rules, users are automatically added or removed based on this criteria. For more information, see the following documentation.

 - [Create or update a dynamic group in Microsoft Entra ID](/entra/identity/users/groups-create-rule)

## Making groups available to AD-based applications
Managing on-premises applications with Active Directory groups that are provisioned from and managed in the cloud used can now be accomplished with Microsoft Entra cloud sync. Now Microsoft Entra cloud sync allows you to fully govern application assignments in AD while taking advantage of Microsoft Entra ID Governance features to control and remediate any access related requests.

For more information, see [Govern on-premises Active Directory based apps (Kerberos) using Microsoft Entra ID Governance](/entra/identity/hybrid/cloud-sync/govern-on-premises-groups).


## Self-service scenarios

:::image type="content" source="media/migrate-entra-id/scenario-1-e.png" alt-text="Conceptual drawing of self-service." lightbox="media/migrate-entra-id/scenario-1-e.png":::

MIM has also been used in self-service scenarios to manage data in Active Directory, for use by Exchange and AD-integrated apps. Now many of these same scenarios can be accomplished from the cloud.

### Self-service group management

You can allow users to create security groups or Microsoft 365 groups/Teams, and then manage the membership of their group.

- [Self-service group management scenarios](/entra/identity/users/groups-self-service-management)

### Access requests with multi-stage approvals

Entitlement management introduces the concept of an access package. An access package is a bundle of all the resources with the access a user needs to work on a project or perform their task, including membership of groups, SharePoint Online sites, or assignment to application roles. Each access package includes policies that specify who gets access automatically, and who can request access.

- [What is entitlement management?](/entra/id-governance/entitlement-management-overview)

### Self-service password reset

Microsoft Entra self-service password reset (SSPR) gives users the ability to change or reset their password. If you have a hybrid environment, you can configure Microsoft Entra Connect to write password change events back from Microsoft Entra ID to an on-premises Active Directory.

- [Self-service password reset](/entra/identity/authentication/overview-authentication#self-service-password-reset)



## Next Steps
- [Identity architecture guides](/azure/architecture/identity/identity-start-here)
- [Resources for managing applications to Microsoft Entra ID](/entra/identity/enterprise-apps/migration-resources)
- [Migrate ADFS applications](/entra/identity/enterprise-apps/migrate-ad-fs-application-howto).
