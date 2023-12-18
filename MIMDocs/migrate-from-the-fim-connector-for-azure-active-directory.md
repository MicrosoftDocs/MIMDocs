---
# required metadata

title: Migrate a Microsoft Entra provisioning scenario from the FIM Connector for Microsoft Entra ID to Microsoft Entra Connect or MIM Graph connector
description: This article documents how customers that had been using the FIM Connector for Microsoft Entra ID could instead use a more recent sync technology or connector.
keywords:
author: markwahl-msft
ms.author: mwahl
manager: amycolannino
ms.date: 8/16/2022
ms.topic: reference
ms.prod: microsoft-identity-manager

ms.assetid:

---

# Migrate a Microsoft Entra provisioning scenario from the FIM Connector for Microsoft Entra ID

While Microsoft Identity Manager (MIM) continues to be supported, one of the connectors, the Forefront Identity Manager (FIM) Connector for Microsoft Entra ID  was [deprecated](microsoft-identity-manager-2016-deprecated-features.md) in 2021.  That solution of using FIM and that connector that had been made available for multi-forest customers in 2014 has been superseded by more recent integration options which provide more hybrid integration features.  If you're still using that connector to provision from FIM or MIM into Microsoft Entra ID, you'll need to update your sync topology to remove that connector and use a different option.  This change is necessary because the internal interfaces used by the Windows Azure AD Connector for FIM are being removed from Microsoft Entra ID, and that connector will no longer be able to connect to Microsoft Entra ID.

Three options to use in place of this connector are:

* Microsoft Entra Connect cloud sync
* Microsoft Entra Connect Sync
* MIM with the Microsoft Graph Connector

Once you've updated your FIM or MIM deployment to one of these options, then the FIM Connector for Microsoft Entra ID should be removed from your FIM or MIM sync engine.  

   > [!NOTE]
   >
   > Customers who plan to change their synchronization technology in a production environment are recommended to work with a partner for help and guidance for this migration.

<a name='migrating-to-azure-ad-connect-cloud-sync'></a>

## Migrating to Microsoft Entra Connect cloud sync

This approach is recommended if you already have one or more Active Directory forests.

In this approach, provisioning would occur in two steps.  You would use MIM sync with the AD MA to provision users or groups into Active Directory. Then, you would use Microsoft Entra Connect cloud sync to bring those objects from that Active Directory into Microsoft Entra ID.  This is because the easiest way to make objects in Active Directory forests available in Microsoft Entra ID is through [Microsoft Entra Connect cloud sync](/azure/active-directory/cloud-sync/what-is-cloud-sync) reading from those forests.  If your forests are in one of the [Microsoft Entra Connect cloud sync supported topologies](/azure/active-directory/cloud-sync/plan-cloud-sync-topologies), then you can [pilot](/azure/active-directory/cloud-sync/tutorial-pilot-aadc-aadccp) a deployment of Microsoft Entra Connect cloud sync for a subset of users in one of those domains.

Once you've completed the migration to Microsoft Entra Connect cloud sync, if FIM or MIM had been used only to provision from an on-premises directory to Microsoft Entra ID, then the FIM or MIM sync engine might no longer be needed.

<a name='migrating-to-azure-ad-connect-sync'></a>

## Migrating to Microsoft Entra Connect Sync

In this approach, you would use MIM sync with the AD MA or Generic LDAP Connector to provision users or groups into an on-premises directory. Then, you would use [Microsoft Entra Connect Sync](/azure/active-directory/hybrid/how-to-connect-install-roadmap) to bring users and groups from that directory into Microsoft Entra ID. This approach is recommended only if your directory topology isn't one of those topologies supported by Microsoft Entra Connect cloud sync. Microsoft Entra Connect Sync has a different list of [supported topologies](/azure/active-directory/hybrid/plan-connect-topologies) than Microsoft Entra Connect cloud sync, and if needed, can be configured with a [non-AD LDAP directory](/azure/active-directory/fundamentals/sync-ldap) as a source.

   > [!NOTE]
   > Deploying the LDAP Connector in Microsoft Entra Connect requires an advanced configuration and this connector is provided under limited support.  Customers who require this configuration in a production environment are recommended to work with a partner such as Microsoft Consulting Services for help, guidance and support for this configuration.

## Migrating to the Microsoft Graph Connector

If you aren't already provisioning users and groups into any on-premises directory, then you may wish to change your MIM sync deployment to use the [Microsoft Identity Manager connector for Microsoft Graph](microsoft-identity-manager-2016-connector-graph.md) instead.  This connector enables integration scenarios for Microsoft Entra ID P1 or P2 customers, for users and groups that aren't in scope of Microsoft Entra Connect cloud sync or Microsoft Entra Connect Sync.  This connector communicates with Microsoft Entra ID via the [Microsoft Graph API](/graph/api/overview) v1.0 and beta.

## Next steps

* [Deprecated features and planning for the future](microsoft-identity-manager-2016-deprecated-features.md)
