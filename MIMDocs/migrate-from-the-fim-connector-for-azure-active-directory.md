---
# required metadata

title: Migrate an Azure AD provisioning scenario from the FIM Connector for Azure AD to Azure AD Connect or MIM Graph connector | Microsoft Docs
description: This article documents how customers that had been using the FIM Connector for Azure AD could instead use a more recent sync technology or connector.
keywords:
author: markwahl-msft
ms.author: mwahl
manager: amycolannino
ms.date: 8/16/2022
ms.topic: reference
ms.prod: microsoft-identity-manager

ms.assetid:

---

# Migrate an Azure AD provisioning scenario from the FIM Connector for Azure AD

While Microsoft Identity Manager (MIM) continues to be supported, one of the connectors, the Forefront Identity Manager (FIM) Connector for Azure Active Directory (Azure AD)  was [deprecated](microsoft-identity-manager-2016-deprecated-features.md) in 2021.  That solution of using FIM and the Azure AD Connector that had been made available for multi-forest customers in 2014 has been superseded by more recent integration options which provide more hybrid integration features.  If you're still using that connector to provision from FIM or MIM into Azure AD, you'll need to update your sync topology to remove that connector and use a different option.  This change is necessary because the internal interfaces used by the Azure AD Connector for FIM are being removed from Azure AD. In the future, that connector will no longer be able to connect to Azure AD.

Three options to use in place of this connector are:

* Azure AD Connect cloud sync
* Azure AD Connect sync
* MIM with the Microsoft Graph Connector

Once you've updated your FIM or MIM deployment to one of these options, then the FIM Connector for Azure AD should be removed from your FIM or MIM sync engine.  

   > [!NOTE]
   >
   > Customers who plan to change their synchronization technology in a production environment are recommended to work with a partner for help and guidance for this migration.

## Migrating to Azure AD Connect cloud sync

This approach is recommended if you already have one or more Active Directory forests.

In this approach, provisioning would occur in two steps.  You would use MIM sync with the AD MA to provision users or groups into Active Directory. Then, you would use Azure AD Connect cloud sync to bring those objects from that Active Directory into Azure AD.  This is because the easiest way to make objects in Active Directory forests available in Azure AD is through [Azure AD Connect cloud sync](/azure/active-directory/cloud-sync/what-is-cloud-sync) reading from those forests.  If your forests are in one of the [Azure AD Connect cloud sync supported topologies](/azure/active-directory/cloud-sync/plan-cloud-sync-topologies), then you can [pilot](/azure/active-directory/cloud-sync/tutorial-pilot-aadc-aadccp) a deployment of Azure AD Connect cloud sync for a subset of users in one of those domains.

Once you've completed the migration to Azure AD Connect cloud sync, if FIM or MIM had been used only to provision from an on-premises directory to Azure AD, then the FIM or MIM sync engine might no longer be needed.

## Migrating to Azure AD Connect sync

In this approach, you would use MIM sync with the AD MA or Generic LDAP Connector to provision users or groups into an on-premises directory. Then, you would use [Azure AD Connect sync](/azure/active-directory/hybrid/how-to-connect-install-roadmap) to bring users and groups from that directory into Azure AD. This approach is recommended only if your directory topology isn't one of those topologies supported by Azure AD Connect cloud sync. Azure AD Connect sync has a different list of [supported topologies](/azure/active-directory/hybrid/plan-connect-topologies) than Azure AD Connect cloud sync, and if needed, can be configured with a [non-AD LDAP directory](/azure/active-directory/fundamentals/sync-ldap) as a source.

   > [!NOTE]
   > Deploying the LDAP Connector in Azure AD Connect requires an advanced configuration and this connector is provided under limited support.  Customers who require this configuration in a production environment are recommended to work with a partner such as Microsoft Consulting Services for help, guidance and support for this configuration.

## Migrating to the Microsoft Graph Connector

If you aren't already provisioning users and groups into any on-premises directory, then you may wish to change your MIM sync deployment to use the [Microsoft Identity Manager connector for Microsoft Graph](microsoft-identity-manager-2016-connector-graph.md) instead.  This connector enables integration scenarios for Azure AD Premium customers, for users and groups that aren't in scope of Azure AD Connect cloud sync or Azure AD Connect sync.  This connector communicates with Azure AD via the [Microsoft Graph API](/graph/api/overview) v1.0 and beta.

## Next steps

* [Deprecated features and planning for the future](microsoft-identity-manager-2016-deprecated-features.md)
