---
# required metadata

title: MIM Deprecated Features And Planning For The Future | Microsoft Docs
description: This article documents deprecated features of the MIM Identity Manager 2016 SP2.
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 8/2/2021
ms.topic: reference
ms.prod: microsoft-identity-manager

ms.assetid:

---

# Deprecated Features

This article describes the deprecated features of Microsoft Identity Manager 2016 SP2. Where the feature is still present in Microsoft Identity Manager, it is still supported, unless it is dependent upon an underlying platform, interface or separate component that is no longer supported. Features are not recommended for new deployments, as they may be removed in a future hotfix or service pack release.  For developers, we recommend not utilizing deprecated features in any new applications or solutions.

> [!NOTE]
>
> For more information on the new support options for MIM, see [support options for Azure AD Premium customers](support-update-for-azure-active-directory-premium-customers.md).

## BHOLD

Microsoft does not recommend customers start new deployments of the Microsoft BHOLD Suite components. Existing deployments of BHOLD will continue to be supported, except where the underlying component is no longer supported. Azure AD now provides [access reviews](/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), which replaces the BHOLD attestation campaign features, and entitlement management, which replaces the access assignment features.

Microsoft Silverlight will reach its end of support on October 12, 2021. For more information, see [Silverlight End of Support](https://support.microsoft.com/windows/silverlight-end-of-support-0a3be3c7-bead-e203-2dfd-74f0a64f1788).
Users who haven't installed Microsoft Silverlight in their browser can't use the BHOLD Suite modules which require Silverlight. This includes the BHOLD Model Generator, BHOLD FIM Self-service integration, and BHOLD Analytics. Customers with an existing BHOLD deployment of one or more of those modules should plan to uninstall those modules from their BHOLD server computers by October 2021. Also, they should plan to uninstall Silverlight from any user computers that were previously interacting with that BHOLD deployment.

## Service and Portal

Deploying MIM Service or Portal on Windows Server 2008 R2, or using SQL Server 2008 R2 as the underlyng database, is deprecated, as these platforms are no longer in mainstream support.  Deploying MIM Portal on SharePoint Foundation 2010 is deprecated.

| **Category**                | **Deprecated Feature**              | **Comment**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programmatic Configuration of sync | Web Service configuration interface(ma-data and mv-data) | The ability to configure the MIM synchronization service, through MIM service web service, may be removed in a future hotfix or service pack.
|

## Connectors and Management Agents

The following MAs have been removed in MIM 2016: </br> 1.  MA for FIM Certificate Management </br>2.  MA for Lotus Notes</br> 3.  MA for SAP R/3 </br> The Lotus Notes and SAP R/3 MAs have been replaced with new connectors. For more information, see [Latest Connector Version Release History & Download](/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history).

The Azure AD Connector for FIM is at feature freeze and deprecated. The solution of using FIM and the Azure AD Connector has been superseded.  Existing deployments should migrate to [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect), Azure AD Connect Sync, or the [Microsoft Graph Connector](microsoft-identity-manager-2016-connector-graph.md), as the internal interfaces used by the Azure AD Connector for FIM are being removed from Azure AD.

## Synchronization Service

Deploying MIM Sync on Windows Server 2008 R2, or using SQL Server 2008 R2 as the underlyng database, is deprecated, as these platforms are no longer in mainstream support.

The ECMA1/XMA extensibility framework has been replaced by ECMA 2.0. Updating existing ECMA1 management agents with ECMA2.0 connectors is required.

| **Category**                | **Deprecated Feature**              | **Comment**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Management Agents           | Running Connectors out-of-proc      | The synchronization service will always call the connector in the same process. It is the responsibility of the connector to start and manage the other process. |
| Management Agents           | Configure partition display name    | This option was only used to provide an alternative name for a partition in the WMI interfaces.                                                                                                                                                                       |
| Run profiles                | Combined profiles                   | The combined profiles delta import/sync, full import/delta sync, and full import/sync may be removed. Use run profiles with two steps instead.

> [!NOTE]
> You should keep combined run profiles only in environments where the performance would be impacted by a large number of existing disconnectors.

| **Category**                | **Deprecated Feature**              | **Comment**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Attribute Precedence | Multi- mastery/equal precedence                       | Equal precedence may be removed. You should configure manual precedence instead. You can continue to use this feature if your environment has a FIM Service management agent deployed. This management agent does not provide manual precedence to avoid export-not-precedent for declarative provisioning. |
| Join Rules           | Join on “Any” object type                             | All join rules should explicitly define the metaverse object type they are trying to join to.       |
| Attribute flows      | Unselect “allow nulls” for exported values            | “Allow Nulls” will always be selected, so make sure that you have “Allow Nulls” selected in your current environment.  |
| Attribute flows      | “Do not recall attributes”                            | Attributes will always be recalled, which is the best practice.  |
| Rules Extension      | Running metaverse and ma rules extension out- of-proc | The metaverse and attribute flow rules will run in the same process as the synchronization engine.       |
| Rules Extension      | Transaction properties                                | Avoid passing data between inbound, provisioning, and outbound synchronization using this utility class.  |
| Rules Extension      | ExchangeUtils: Create55\* methods                     | The methods to create objects for Exchange 5.5 servers may be removed.        |
| Interface            | Mms_Metaverse                                        | All ClmUtils class members may be removed in a future hotfix or service pack.   |

## Certificate Management

Deploying MIM CM on Windows Server 2008 R2, or using SQL Server 2008 R2 as the underlyng database, is deprecated.

The MIM CM bulk client is deprecated and not recommended for new deployments.

## MIM PAM

Deploying MIM for Privileged Access Manager with a Windows Server 2012 R2 domain controller in the PRIV forest is deprecated.  Use Windows Server 2016 or later Active Directory, with WIndows Server 2016 functional level, for your PRIV forest domain.  (Windows Server 2012 R2 is permtted for a CORP forest domain.)

## Next steps
Learn more about:

- Microsoft Identity Manager is still closely related to its predecessor, Forefront Identity Manager. If you still use FIM, or want to refer to additional documentation, take a look at the [FIM 2010 R2 Documentation Roadmap](https://technet.microsoft.com/library/jj133885.aspx).
- [Topology considerations for deploying MIM](topology-considerations.md) This article introduces multiple deployment topologies that you may consider implementing.
- [Capacity planning guide](capacity-planning-guide.md) You can use this guide, along with test environments, to understand the appropriate scope for your deployment.

