---
# required metadata

title: Migrating from the FIM Connector for Azure AD | Microsoft Docs
description: This article documents options for customers that had been using the FIM Connector for Azure AD.
keywords:
author: markwahl-msft
ms.author: mwahl
manager: billmath
ms.date: 8/16/2022
ms.topic: reference
ms.prod: microsoft-identity-manager

ms.assetid:

---

# Migrating from the FIM Connector for Azure AD

While Microsoft Identity Manager (MIM) continues to be supported, the Forefront Identity Manager (FIM) Connector for Azure Active Directory (Azure AD) from 2014 was [deprecated](microsoft-identity-manager-2016-deprecated-features.md) in 2021.  The solution of using FIM and the Azure AD Connector has been superseded by more recent integration options.  If you were using the Windows Azure Active Directory connector to provision from FIM or MIM into Azure AD, you will need to update your sync topology to remove that connector.  This change is necessary because the internal interfaces used by the Azure AD Connector for FIM are being removed from Azure AD, and in the future, the Windows Azure AD Connector will no longer be able to connect to Azure AD.

Three options to use in place of this connector are:

* Azure AD Connect cloud sync
* Azure AD Connect sync
* MIM with the Microsoft Graph Connector

Once you have updated your deployment to one of these options, then the FIM Connector for Azure AD should be removed from your FIM or MIM sync engine.  

## Migrating to Azure AD Connect cloud sync or Azure AD Connect sync

In this approach, MIM would use the AD MA or Generic LDAP Connector to provision users or groups from the metaverse into an on-premises directory, and rely upon other synchronization agents to bring those objects from that directory into Azure AD.  If FIM or MIM was solely being used to provision from an on-premises directory to Azure AD, then the sync engine might no longer be needed after that agent is deployed.

The easiest way to make objects in Active Directory forests available in Azure AD is through [Azure AD Connect cloud sync](/active-directory/cloud-sync/what-is-cloud-sync) reading from those forests.  If your forests are in one of the [Azure AD Connect cloud sync supported topologies](/azure/active-directory/cloud-sync/plan-cloud-sync-topologies), then you can [pilot](/azure/active-directory/cloud-sync/tutorial-pilot-aadc-aadccp) a deployment of Azure AD Connect cloud sync for a limited number of users or groups in one of those domains.

If your directory topology is not one of those covered by Azure AD Connect cloud sync, but is one of the [Azure AD Connect sync supported topologies](/azure/active-directory/hybrid/plan-connect-topologies), or you have a [non-AD LDAP directory](/azure/active-directory/fundamentals/sync-ldap), then you could use [Azure AD Connect sync](/azure/active-directory/hybrid/how-to-connect-install-roadmap) to bring those users from your directory into Azure AD.

## Migrating to the Microsoft Graph Connector

If your users and groups are not already provisioned into any on-premises directory, then you may wish to change to the [Microsoft Identity Manager connector for Microsoft Graph](microsoft-identity-manager-2016-connector-graph.md).  This connector enables additional integration scenarios for Azure AD Premium customers, for users and groups that are not in scope of Azure AD Connect cloud sync or Azure AD Connect sync.  It surfaces in the MIM sync metaverse additional objects obtained from the [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 and beta.

## Next steps

* [Deprecated features and planning for the future](microsoft-identity-manager-2016-deprecated-features.md)
