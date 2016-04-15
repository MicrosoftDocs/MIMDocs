---
# required metadata

title: Identity Manager Hybrid Reporting in Azure | Microsoft Identity Manager
description: Azure Active Directory hybrid reporting lets you create custom reports that include both cloud and on-premises events.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Identity Manager Hybrid Reporting in Azure
With Azure Active Directory (AD) you can create a single report to view identity management activity that happened either on-premises or in the cloud. This reporting capability gives you a consolidated place from which to manage identity and access data, while reducing overall costs.

## What is Azure AD hybrid reporting?
Hybrid reporting helps IT professionals address common identity management reporting challenges.

1. **Collect identity management activities across different systems.** Hybrid reports show you identity management activity from Azure AD and Identity Manager.

2. **Export reporting data and create custom reports.** In addition to viewing your reports in the Azure portal, you can also export the data to generate your own custom views.

3. **Reduce reporting system infrastructure cost.** Hybrid reporting in the cloud means you can eliminate on-premises reporting data-warehouse infrastructure.

## How does it work?

To collect the on-premises data, you first install a reporting agent on your Identity Manager server. The reporting agent is downloaded from the configure page of your directory in the [Azure classic portal](https://manage.windowsazure.com/).

The process of hybrid reporting follows these steps:
1. After the reporting agent is installed, the Identity Manager activity data is sent to the Windows Event Log.
2. The reporting agent processes the events in the Windows Event Log and uploads them to Azure.
3. The activity data is stored in Azure for one month.
4. When you request a report, the activity events are parsed and filtered for the required reports.
5. The Azure classic portal retrieves the reporting data and renders this as the activity report.

## See also
- Get more details about [Working with Identity Manager Hybrid Reporting](/microsoft-identity-manager/DeployUse/working-with-identity-manager-hybrid-reporting)
