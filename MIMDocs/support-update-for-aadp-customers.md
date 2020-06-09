---
# required metadata

title: Support update for Azure AD Premium customers using Microsoft Identity Manager | Microsoft Docs
description: This article describes how Azure AD Premium customers can get support after 1/21/2021.
keywords:
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 6/9/2021
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

# Support update for Azure AD Premium customers

Applies to: Azure AD Premium, Microsoft Identity Manager (MIM)

For Azure AD Premium customers, standard support for specific components of Microsoft Identity Manager Service Pack 2 or later that enable Azure AD integration is available from June 2020 onward, continuing after January 2021.

This gives customers more time for their migration to Azure AD. The MIM components for which standard support is available include:
- Synchronization Service
- Service and Portal
- Connectors
- Password Change Notification Service (PCNS)
- Add-ins and extensions
- Data Warehouse Support Scripts
- Language packs

These components enable customers to populate Active Directory, and by extension, Azure AD through Azure AD Connect, with the users and groups provisioned from an on-premises HR system or other system of record.

## What is the updated support?

Azure AD Premium customers are able to receive through Azure standard support, support for the above mentioned components of Microsoft Identity Manager 2016 Service Pack 2, or a later hotfix or update.

## How would an Azure AD Premium customer obtain support for these MIM components?

A customer can create an Azure support request, using the instructions at https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request. 

If using the Azure portal:
 - select *Issue type: Technical*
 - switch to show *All Services* 
 - in the services list under *Azure Active Directory* select  *User Provisioning and Synchronization*
 - select *Problem type: Microsoft Identity Manager (MIM)*
 - select *Problem subtype*: *Connectors*, *Service and Portal* or *Synchronization engine*
 
![Create MIM Support request](media/aad-new-support-request.png)

## When can Azure AD Premium customers start receiving standard support for these MIM components?

The MIM components are listed in the Azure portal services list as of June 2020.

## For which subscriptions suites is this applicable?

A suite that fulfills Azure AD Premium Plan 1 or Plan 2. As of June 2020, this includes Enterprise Mobility + Security E3 or E5, Microsoft 365 Enterprise F1, E5, or E5 Security. Other suites may also meet the requirements – for a complete list, consult the Microsoft Online Services Terms at https://www.microsoft.com/licensing/product-licensing/products. 

## What MIM components does this cover?

Standard support is available for Azure AD Premium customers using following components of Microsoft Identity Manager 2016 Service Pack 2, or a later hotfix or update: Synchronization Service, Password Change Notification Service (PCNS), Connectors, Service and Portal, Add-ins and extensions, Data Warehouse Support Scripts, and language packs.

## What about other MIM components?

For the other MIM components not listed above, including Certificate Management (CM), CM Bulk Client, CM Client, CM App for Windows, hybrid reporting agent, PAM and BHOLD, the fixed and service pack lifecycle policy applies. This is described at https://support.microsoft.com/lifecycle/search?alpha=microsoft%20identity%20manager%202016  

## What versions of MIM does this cover?

This is applicable to customers who are using Microsoft Identity Manager 2016 Service Pack 2, or a [later hotfix or update](reference/version-history.md). *(MIM 2016 SP2, build 4.6.34.0, was released in October 2019).* Customers are highly encouraged to stay on a fully supported service pack to ensure they are on the latest and most secure version of their product. For customers still using an older build of MIM, see the service pack lifecycle policy at https://support.microsoft.com/help/17138.