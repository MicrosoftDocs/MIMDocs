---
title: Use Azure Monitor for Microsoft Identity Manager reporting
description: Get the steps to configure Azure Monitor with MIM
services: active-directory
documentationcenter: ''
keywords: MIM
author: billmath
ms.author: billmath
reviewer: markwahl-msft
manager: amycolannino
ms.date: 11/04/2024
ms.topic: article
ms.service: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
---

# Microsoft Identity Manager 2016 reporting with Azure Monitor
[Azure Monitor](/azure/azure-monitor/overview) is a monitoring solution for collecting, analyzing, and responding to monitoring data from your cloud and on-premises environments. MIM Synchronization Service writes to the event log for key events, and the MIM Service can be configured to add records to a Windows event log for requests it receives. These event logs are transported by Azure Arc to Azure Monitor, and can be retained in an Azure Monitor workspace alongside the Microsoft Entra audit log, and logs from other [data sources](/azure/azure-monitor/data-sources). You can then use [Azure Monitor workbooks](/azure/azure-monitor/visualize/workbooks-overview) to format the MIM events in a report, and [alerts](/azure/azure-monitor/alerts/alerts-overview) to monitor for specific events in MIM Service.

Setting up Azure Monitor with your MIM server consists of the following steps:

 1. [Join MIM servers to Azure with Azure Arc](#join-mim-server-to-azure-with-azure-arc)
 2. [Install the Azure Monitor extensions](#install-the-azure-monitor-extensions)
 3. [Create a workspace](#create-a-data-collection-rule)
 4. [Create a Data Collection Rule (DCR)](#create-a-data-collection-rule)
 5. [Verify the MIM data](#verify-data)


 The following sections describe each of the individual steps.

## Prerequisties
You should make sure that you meet the Azure Arc and Azure Monitor prerequisites before attempting the steps outlined below.

- [Azure Arc Prerequisites](/azure/azure-arc/servers/plan-at-scale-deployment#prerequisites)
- [Collect Windows events with Azure Monitor Agent - Prerequisites](/azure/azure-monitor/agents/data-collection-windows-events#prerequisites)

Also, a resource group in Azure is required before joining the server with Azure Arc. If you do not have a resource group, you can [create one](/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups) before generating the Azure Arc installation script.

## Join MIM server to Azure with Azure Arc
You'll likely have one or more Windows Server machines that run MIM Sync or MIM Service in your environment, potentially located on-premises. To join any non-Azure hosted Windows Server to Azure, you generate a script and run it locally on each of those servers. This provides a consistent management experience across native Azure virtual machines and servers anywhere. When a non-Azure machine is Arc-enabled, it becomes a connected machine and is treated as a resource in Azure, with its own resource Id and projection in Azure. 

To join your MIM server, you generate a script and run it locally on the MIM server. Follow the prompts in the portal to create the script. Download the script and run it on the MIM server. After the script completes, the MIM server should appear under Azure Arc in the portal.

:::image type="content" source="media/mim-azure-monitor-reporting/azure-monitor-1.png" alt-text="Screenshot Azure Arc." lightbox="media/mim-azure-monitor-reporting/azure-monitor-1.png":::
 

For more information, see [Connect Windows Server machines to Azure mim-azure-monitor-reportingthrough Azure Arc Setup](/azure/azure-arc/servers/onboard-windows-server)


## Install the Azure Monitor extensions
After you've joined the Windows Server machines, which have MIM Sync or MIM Service installed, to Azure, you can use the Azure Monitor agent on those servers to begin collecting Windows Event logs. Azure Arc-enabled servers support the Azure VM extension framework, which provides post-deployment configuration and automation tasks, enabling you to simplify management of your hybrid machines like you can with Azure VMs.

After you have MIM joined to Azure, you can the Azure Monitor agent on the MIM server to beginning collecting Windows Event data. To install the Azure Monitor extensions you can use the following PowerShell script. Be sure to replace the variables with your information. 

```PowerShell
## Install the Azure Monitor Agent
Install-Module -Name Az.ConnectedMachine
$subscriptionID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" 
$tenantID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
$resourcegroup = "MIM-resource-group"
$MIMServer = "MIM"
$location = eastus
Connect-AzAccount -Tenant $tenantID -SubscriptionId $subscriptionID 
New-AzConnectedMachineExtension -Name AzureMonitorWindowsAgent -ExtensionType AzureMonitorWindowsAgent -Publisher Microsoft.Azure.Monitor -ResourceGroupName $resourcegroup -MachineName $MIMServer -Location $location -EnableAutomaticUpgrade
```

For more information, see [Deployment options for Azure Monitor agent on Azure Arc-enabled servers](/azure/azure-arc/servers/concept-log-analytics-extension-deployment)

## Create a workspace 
A Log Analytics workspace is a data store into which you can collect any type of log data from all of your Azure and non-Azure resources and applications.

Before we create a data collection rule that collects the Windows Event log information, we need somewhere to send this information. Follow the steps outline in [Create a workspace](/azure/azure-monitor/logs/quick-create-workspace?tabs=azure-portal#create-a-workspace) to create a Log Analytics workspace.

## Create a Data Collection Rule
Data collection rules (DCRs) are part of an Extract, Transform, and Load (ETL) data collection process that improves on legacy data collection methods for Azure Monitor. This process uses a common data ingestion pipeline, the Azure Monitor pipeline, for all data sources and a standard method of configuration that's more manageable and scalable than other methods.

To create the data collection rule for the MIM server. Use the following steps.

1. On the Monitor home screen in the Azure portal, select **Settings** and **Data Collection Rules**.
2. At the top, click **Create**
3. Give your rule a name, associate it with your resource group, and the region your resource group is located in.
4. Click **Next**
5. On the resources tab, click **Add resources** and under your resource group, add the MIM server. Click **Next**.
6. On **Collect and deliver** and the **Windows Event Logs** as the data source.
7. On **Basic** you can add the basic Windows Event logs, System, Security, and Application.
8. Click on **Custom**.
9. Enter the following in the box under **Use XPath queries to filter event logs and limit data collection**

|Xpath query|Description|
|-----|-----|
|Forefront Identity Manager!*[System[(Level=1 or Level=2 or Level=3 or Level=4 or Level=0 or Level=5)]]| The MIM service log.
|Forefront Identity Manager Management Agent!*[System[(Level=1 or Level=2 or Level=3 or Level=4 or Level=0 or Level=5)]]|The MIM management agent log|
|Forefront Identity Manager Synchronization%4Operational!*[System[(Level=1 or Level=2 or Level=3 or Level=4 or Level=0 or Level=5)]]|The operations log for the MIM synchronization engine|

:::image type="content" source="media/mim-azure-monitor-reporting/azure-monitor-2.png" alt-text="Screenshot of data collection sources." lightbox="media/mim-azure-monitor-reporting/azure-monitor-2.png":::

10. Click **Next Destination** and click **Add Destination**
11. Enter the following:
  - Destination Type: Azure Monitor Logs
  - Subscription: Your subscription
  - Destination Details: Your workgroup

12. Click **Add data source**
13. Click **Review and Create**
14. Click **Create**

Once the DCR is created and deployed, event log information begins to flow from the MIM server.

## Verify data
To verify that you are collecting data, you can go to your workspace and run the following query.

1. On your workspace, select logs
2. Enter the following query: `Event | where TimeGenerated > ago(48h)`
3. You should see your MIM data.
 
 :::image type="content" source="media/mim-azure-monitor-reporting/azure-monitor-3.png" alt-text="Screenshot of data collected." lightbox="media/mim-azure-monitor-reporting/azure-monitor-3.png":::

## Create a workbook for your data
Workbooks provide a flexible canvas for data analysis and the creation of rich visual reports within the Azure portal. Now that the MIM data is in the portal, you can use workbooks. Workbooks let you combine multiple kinds of visualizations and analyses, making them great for freeform exploration.

For more information, see [Create or edit an Azure Workbook](/azure/azure-monitor/visualize/workbooks-create-workbook)