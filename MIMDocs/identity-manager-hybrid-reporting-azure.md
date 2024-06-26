---
# required metadata

title: What is hybrid reporting in Microsoft Entra ID?
description: Hybrid audit activity reports in Microsoft Entra ID lets you view audited events in both the cloud and on-premises.
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 09/14/2023
ms.topic: article
ms.service: microsoft-identity-manager

ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Hybrid identity management audit reporting in Microsoft Entra ID

With Microsoft Entra audit activity reporting, you can monitor identity management activity either on-premises or in the cloud. By managing all your identity and access data in a single report, you can save time and reduce overall costs.

<a name='what-is-azure-active-directory-hybrid-reporting'></a>

## What is Microsoft Entra hybrid reporting?

Hybrid audit reporting helps IT professionals address common identity-management reporting challenges, such as:

- **Collecting identity management activities across different systems**. Hybrid reports show you identity management activity from Microsoft Entra ID and Identity Manager.

- **Exporting reporting data and creating custom reports**. In addition to viewing your reports in the Azure portal, you can export the data to generate your own custom views.

- **Reducing reporting system infrastructure cost**. Hybrid reporting in the cloud means you can help eliminate the costs that are associated with your on-premises, data-warehouse infrastructure.

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
- [Audit activity reports in the Microsoft Entra admin center](/azure/active-directory/reports-monitoring/concept-audit-logs)
- [Reporting retention policies](/azure/active-directory/reports-monitoring/reference-reports-data-retention)
- [Microsoft Azure log integration (SIEM)](/previous-versions/azure/security/fundamentals/azure-log-integration-overview)
- [Microsoft Entra reporting API](/azure/active-directory/reports-monitoring/concept-reporting-api)
