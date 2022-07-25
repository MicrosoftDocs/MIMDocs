---
# required metadata

title: Support update for Azure AD Premium customers using Microsoft Identity Manager | Microsoft Docs
description: This article describes how Azure AD Premium customers can get support after January 21, 2021.
keywords:
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 6/9/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
reviewer: markwahl-msft
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Support update for Azure AD Premium customers using Microsoft Identity Manager

Applies to: Azure AD Premium, Microsoft Identity Manager (MIM)

For Azure AD Premium customers, standard support is available from June 2020 onward, continuing after January 2021, for specific components of [Microsoft Identity Manager 2016 Service Pack 2](/microsoft-identity-manager/microsoft-identity-manager-2016), or later service packs, that enable Azure AD integration. This is in addition to the existing support for Microsoft Identity Manager already provided through the [fixed lifecycle policy](/lifecycle/policies/fixed) and plans for [support for businesses](https://support.microsoft.com/help/4341255).

The MIM components for which standard support is available include:
- MIM Synchronization Service and Password Change Notification Service (PCNS)
- MIM Service and Portal, Add-ins and extensions, Data Warehouse Support Scripts and Language packs
- MIM Connectors

These MIM components populate Active Directory, and by extension, Azure AD through Azure AD Connect, with the users and groups provisioned from an on-premises HR system or other system of record sources. This ensures customers using Azure AD Premium with on-premises systems can continue to be supported during the migration of their identity management scenarios from on-premises systems to Azure AD. 

## Opening a support request in the Azure portal

As an additional support option for Microsoft Identity Manager, Azure AD Premium customers are able to request support for the above mentioned components of Microsoft Identity Manager 2016 Service Pack 2, or a later hotfix or update, through the Azure portal.

A customer can create an Azure support request, using the instructions at [How to create an Azure support request](/azure/azure-portal/supportability/how-to-create-azure-support-request):
1. select *Issue type: Technical*
1. switch to show *All Services*
1. in the services list under Azure Active Directory selects  *User Provisioning and Synchronization*
1. select *Problem type: Microsoft Identity Manager (MIM)*
1. select *Problem subtype*: *Connectors*, *Service and Portal* or *Synchronization engine*

![Create MIM Support request](media/azure-active-directory-new-support-request.png)

The MIM components are listed as problem types within *Azure Active Directory User Provisioning and Synchronization* in the Azure portal.

For requests opened through the Azure portal, standard support is available for Azure AD Premium customers for following components of Microsoft Identity Manager 2016 Service Pack 2, or a [later hotfix or update](reference/version-history.md): Synchronization Service, Password Change Notification Service (PCNS), Connectors, Service and Portal, Add-ins and extensions, Data Warehouse Support Scripts, and language packs.

## Other support options

MIM 2016 SP2, build 4.6.34.0, was released in October 2019. Customers are highly encouraged to stay on a fully supported service pack to ensure they are on the latest and most secure version of their product. For more information, see the [service pack lifecycle policy](https://support.microsoft.com/help/17138).

For customers still using an older build of MIM, or customers who do not have Azure support or subscription to a suite that includes Azure Active Directory Premium, or for issues with other components of MIM not listed above, support continues to be available. The support policy is described at [Fixed lifecycle policy](/lifecycle/policies/fixed) with the specific dates at [support lifecycle for Microsoft Identity Manager 2016](https://support.microsoft.com/lifecycle/search?alpha=microsoft%20identity%20manager%202016).

In addition to Azure support, there are several other support options organizations can use to obtain support. For example, if you have Microsoft Professional Support, you can [create a new support request](https://support.microsoft.com/supportforbusiness/productselection). To select the relevant MIM component:
1. select product family *Security*
1. select product *Identity Manager 2016*
