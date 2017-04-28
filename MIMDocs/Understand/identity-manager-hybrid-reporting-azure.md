---
# required metadata

title: What is hybrid reporting | Microsoft Docs
description: Hybrid Audit Activity reports in Azure Active Directory lets you view both cloud and on-premises audited events.
keywords:
author: fimguy
ms.author: fimguy
manager: femila
ms.date: 04/28/2017
ms.topic: article
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

# Hybrid identity management Audit reports in Azure Active Directory - Public Preview(Refresh)
With Azure Active Directory (AD) Audit Activity reports you can view single report to monitor identity management activity that happens either on-premises or in the cloud. This feature lets you manage all of your identity and access data in one place,  saving time and reducing overall costs.

## What is Azure Active Directory hybrid reporting?
Hybrid audit reporting helps IT professionals address common identity management reporting challenges.

1. **Collect identity management activities across different systems.** Hybrid reports show you identity management activity from Azure AD and Identity Manager.

2. **Export reporting data and create custom reports.** In addition to viewing your reports in the Azure portal, you can also export the data to generate your own custom views.

3. **Reduce reporting system infrastructure cost.** Hybrid reporting in the cloud means you could eliminate on-premises reporting data-warehouse infrastructure.

## How does it work?

To collect the on-premises data, you first install a reporting agent on your Identity Manager 2016 server. The reporting agent is downloaded from the Microsoft Download Page [here](https://www.microsoft.com/en-us/download/details.aspx?id=55112).

The process of hybrid reporting follows these steps:
1. After the reporting agent is installed, the Identity Manager activity data is sent to the Windows Event Log.
2. The reporting agent processes the delta events every 10 min or on service restart in the Windows Event Log and uploads them to Azure Portal.
3. Azure Portal processes recieved data within 1 hour of data recieved
4. The activity data is stored in Azure for one month.
5. The Azure portal retrieves the audit reporting data and renders this as the audit within the Azure Audit Reporing Blade.

## See also
- Get more details about [Working with Identity Manager Hybrid Reporting](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)
- Get more details about [Audit activity reports in the Azure Active Directory portal](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)