---
# required metadata

title: MIM Deprecated Features And Planning For The Future
description: This article documents deprecated features of the MIM Identity Manager 2016 SP2.
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 11/19/2024
ms.topic: reference
ms.service: microsoft-identity-manager

ms.assetid:

---

# Deprecated Features and planning for the future

This article describes the deprecated features of Microsoft Identity Manager 2016 SP2. Where the feature is still present in Microsoft Identity Manager, it's still supported, except where the feature is dependent upon an underlying platform, interface, or separate component that is no longer supported. Deprecated features aren't recommended for new deployments, as they may be removed in a future hotfix or service pack release. For developers, we recommend not utilizing deprecated features in any new applications or solutions.

> [!NOTE]
>
> For more information on the new support options for MIM, see [support options for Microsoft Entra ID P1 or P2 customers](support-update-for-azure-active-directory-premium-customers.md).

## BHOLD

Microsoft doesn't recommend customers start new deployments of the Microsoft BHOLD Suite components. For some modules, the underlying component is no longer supported.

The BHOLD Model Generator, BHOLD Analytics, and BHOLD FIM Integration modules have a dependency on Microsoft Silverlight. Microsoft Silverlight reached its end of support on October 12, 2021. For more information, see [Silverlight End of Support](https://support.microsoft.com/windows/silverlight-end-of-support-0a3be3c7-bead-e203-2dfd-74f0a64f1788). Those BHOLD Suite modules that required Silverlight should no longer be used. Customers with an existing BHOLD deployment of one or more of those modules should uninstall those modules from their BHOLD server computers. Also, they should uninstall Silverlight from any user computers that were previously interacting with that BHOLD deployment.

Microsoft Entra ID now provides [access reviews](/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), which replaces the BHOLD attestation campaign features, and entitlement management, which replaces the access assignment features.

## Service and Portal

Don't deploy MIM Service or Portal on Windows Server 2008 R2, or use SQL Server 2008 R2 as the underlying database, as these platforms are no longer in mainstream support. Deploying MIM Portal on SharePoint Foundation 2010 is deprecated.

| **Category**                | **Deprecated Feature**              | **Comment**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programmatic Configuration of sync | Web Service configuration interface(ma-data and mv-data) | The ability to configure the MIM synchronization service, through MIM service web service, may be removed in a future hotfix or service pack.
|

<a name='azure-ad-multi-factor-authentication-integration'></a>

## Microsoft Entra multifactor authentication integration


> [!IMPORTANT]
> In September 2022, Microsoft announced deprecation of Azure Multi-Factor Authentication Server. Beginning September 30, 2024, Azure Multi-Factor Authentication Server deployments no longer service multifactor authentication (MFA) requests. Customers of Azure Multi-Factor Authentication Server, for MIM SSPR or MIM PAM approvals, must move to instead use either custom MFA providers, or Windows Hello or smartcard-based authentication in AD.

## Hybrid reporting

The MIM hybrid reporting feature, introduced with Microsoft Identity Manager (MIM) 2016, is deprecated. This feature allowed the MIM hybrid reporting agent to send event logs from the MIM service to Microsoft Entra, enabling reports for password reset using self-service password reset (SSPR) and self-service group management (SSGM) in the Microsoft Entra audit log. This is replaced by using Azure Arc agent to send those event logs to Azure Monitor, as this allows more flexible reports. As of November 2025, the cloud endpoints used by the MIM hybrid reporting agent will no longer be available, and customers should transition to Azure Monitor or similar. Other MIM and Entra ID Connect Health capabilities are unaffected by this deprecation.

For more information, see [Microsoft Identity Manager 2016 reporting with Azure Monitor](mim-azure-monitor-reporting.md).

## Connectors and Management Agents

The following MAs were removed in MIM 2016:

- MA for FIM Certificate Management
- MA for Lotus Notes
- MA for SAP R/3
- The Windows Azure AD Connector for FIM

The Lotus Notes and SAP R/3 MAs were replaced with new connectors. For more information, see [Latest Connector Version Release History & Download](/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history).


## Synchronization Service

Deploying MIM Sync on Windows Server 2008 R2, Windows Server 2012 or Windows Server 2012 R2, or using SQL Server 2008 R2 as the underlying database, is deprecated, as these platforms are no longer in mainstream support.

The ECMA1/XMA extensibility framework has been replaced by ECMA 2.0. Updating existing ECMA1 management agents with ECMA2.0 connectors is required.

| **Category**                | **Deprecated Feature**              | **Comment**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Management Agents           | Running Connectors out-of-proc      | The synchronization service will always call the connector in the same process. It's the responsibility of the connector to start and manage the other process. |
| Management Agents           | Configure partition display name    | This option was only used to provide an alternative name for a partition in the WMI interfaces.                                                                                                                                                                       |
| Run profiles                | Combined profiles                   | The combined profiles delta import/sync, full import/delta sync, and full import/sync are no longer supported.

> [!NOTE]
> You should keep combined run profiles only in environments where the performance would be impacted by a large number of existing disconnectors.

| **Category**                | **Deprecated Feature**              | **Comment**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Attribute Precedence | Multi- mastery/equal precedence                       | Equal precedence may be removed. You should configure manual precedence instead. You can continue to use this feature if your environment has a FIM Service management agent deployed. This management agent doesn't provide manual precedence to avoid export-not-precedent for declarative provisioning. |
| Join Rules           | Join on "Any" object type                             | All join rules should explicitly define the metaverse object type they're trying to join to.       |
| Attribute flows      | Unselect "allow nulls" for exported values            | "Allow Nulls" will always be selected, so make sure that you have "Allow Nulls" selected in your current environment.  |
| Attribute flows      | "Don't recall attributes"                            | Attributes will always be recalled, which is the best practice.  |
| Rules Extension      | Running metaverse and ma rules extension out- of-proc | The metaverse and attribute flow rules will run in the same process as the synchronization engine.       |
| Rules Extension      | Transaction properties                                | Avoid passing data between inbound, provisioning, and outbound synchronization using this utility class.  |
| Rules Extension      | ExchangeUtils: Create55\* methods                     | The methods to create objects for Exchange 5.5 servers may be removed.        |
| Interface            | Mms_Metaverse                                        | All ClmUtils class members may be removed in a future hotfix or service pack.   |

## Certificate Management

Don't deploy MIM CM on Windows Server 2008 R2, or use SQL Server 2008 R2 as the underlying database, as those platforms are out of support.

The MIM CM bulk client is not recommended for new deployments.

## MIM PAM

The PAM approach provided by MIM is intended to be used in a custom architecture for isolated environments where Internet access is not available, where this configuration is required by regulation, or in high impact isolated environments like offline research laboratories and disconnected operational technology or supervisory control and data acquisition environments. PAM is not recommended as a starting point in deployments of Active Directory with Internet connectivity. If your Active Directory is part of an Internet-connected environment, see [securing privileged access](/security/compass/overview) on where to start.

Deploying MIM for Privileged Access Management with a Windows Server 2012 R2 domain controller in the `PRIV` forest is deprecated. Use Windows Server 2016 or later Active Directory, with Windows Server 2016 functional level, for your `PRIV` forest domain. The Windows Server 2012 R2 functional level is still permitted for a `CORP` forest's domain.

## Next steps
Learn more about:

- Microsoft Identity Manager is still closely related to its predecessor, Forefront Identity Manager. Additional documentation is in the [FIM 2010 R2 Documentation Roadmap](https://technet.microsoft.com/library/jj133885.aspx).
- [Topology considerations for deploying MIM](topology-considerations.md) This article introduces multiple deployment topologies that you may consider implementing.
- [Capacity planning guide](capacity-planning-guide.md) You can use this guide, along with test environments, to understand the appropriate scope for your deployment.
