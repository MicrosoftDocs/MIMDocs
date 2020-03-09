---
# required metadata

title: What is hybrid reporting in Azure AD? | Microsoft Docs
description: Hybrid audit activity reports in Azure Active Directory lets you view audited events in both the cloud and on-premises.
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Hybrid identity management audit reporting in Azure Active Directory
With Azure Active Directory (Azure AD) audit activity reporting, you can monitor identity management activity either on-premises or in the cloud. By managing all your identity and access data in a single report, you can save time and reduce overall costs.

## What is Azure Active Directory hybrid reporting?
Hybrid audit reporting helps IT professionals address common identity-management reporting challenges, such as:

* **Collecting identity management activities across different systems**. Hybrid reports show you identity management activity from Azure AD and Identity Manager.

* **Exporting reporting data and creating custom reports**. In addition to viewing your reports in the Azure portal, you can export the data to generate your own custom views.

* **Reducing reporting system infrastructure cost**. Hybrid reporting in the cloud means you can help eliminate the costs that are associated with your on-premises, data-warehouse infrastructure.

## How does it work?

To collect the on-premises data, you first install a reporting agent on your Identity Manager 2016 server. [Download the Microsoft Identity Manager Hybrid Reporting Agent](https://www.microsoft.com/download/details.aspx?id=55112).

Hybrid reporting undergoes the following process:
1. After you install the reporting agent, the Identity Manager activity data is sent to Windows Event Log.
2. The reporting agent processes the delta events every 10 minutes or when the Windows Event Log service restarts. The agent then uploads the events to the Azure portal.
3. The Azure portal processes the received data within one hour of receiving it.
4. The activity data is stored in Azure for one month.
5. The Azure portal retrieves the audit reporting data and displays it in the Azure Audit Reporting window.

## Next steps
Learn more about:
- [Working with Identity Manager Hybrid Reporting](working-with-identity-manager-hybrid-reporting.md)
- [Audit activity reports in the Azure Active Directory portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Reporting retention policies](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Microsoft Azure log integration (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Azure Active Directory reporting API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
