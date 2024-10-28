title: Use Azure Monitor for Microsoft Identity Manager reporting
description: Get the steps to configure Azure Monitor with MIM
services: active-directory
documentationcenter: ''
keywords: MIM
author: billmath
ms.author: billmath
reviewer: markwahl-msft
manager: amycolannino
ms.date: 10/23/2024
ms.topic: article
ms.service: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity

# Azure Monitor for Microsoft Identity Manager reporting
The following document outlines how you can use Azure Monitor for reporting with Microsoft Identity Manager.

Setting up Azure Monitor with your MIM server consists of the following steps:

 1. [Join MIM to Azure with Azure Arc](#join-mim-server-to-azure-with-azure-arc)
 2. [Install the Azure Monitor extensions](#install-the-azure-monitor-extensions)
 3. [Create a Data Collection Rule (DCR)](#create-a-data-collection-rule)
 4. [Associate the DCR with a workspace](#associate-dcr-with-workspace)
 5. [Verify the MIM data](#verify-data)
 6. [Use the data to create workbooks and reports] 

 The sections below describe each of the individual steps.

## Prerequisties
Familiarize your self with the Azure Arc and Azure Monitor prerequisites prior to attmepting the steps outlined below.

- [Azure Arc Prerequisties](/azure/azure-arc/servers/plan-at-scale-deployment#prerequisites)
- [Collect Windows events with Azure Monitor Agent - Prerequisites](/azure/azure-monitor/agents/data-collection-windows-events#prerequisites)

Also, you will need a resource group in Azure before joining the server with Azure Arc.  If you do not have a resouce group, you can [create one](/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups) prior to generating the Azure Arc installation script.

## Join MIM server to Azure with Azure Arc
Azure Arc-enabled servers enables you to manage your Windows and Linux physical servers and virtual machines hosted outside of Azure: on-premises, other cloud providers, and edge environments. This provides a consistent management experience across native Azure virtual machines and servers anywhere. When a non-Azure machine is Arc-enabled, it becomes a connected machine and is treated as a resource in Azure, with its own resource Id and projection in Azure. 

To join your MIM server, you will need to generate a script.  Follow the prompts in the portal to create the script.  Download the script and run it on the MIM server.  This will join the server to Azure.

:::image type="content" source="media/mim-azure-monitor-reporting/azure-reporting-1.png" alt-text="Screenshot Azure Arc." lightbox="media/mim-azure-monitor-reporting/azure-reporting-1.png":::
 

For more information see [Connect Windows Server machines to Azure through Azure Arc Setup](/azure/azure-arc/servers/onboard-windows-server)


## Install the Azure Monitor extensions
Azure Monitor supports multiple methods to install the Azure Monitor agent and connect your machine or server registered with Azure Arc-enabled servers to the service. Azure Arc-enabled servers support the Azure VM extension framework, which provides post-deployment configuration and automation tasks, enabling you to simplify management of your hybrid machines like you can with Azure VMs.

After you have MIM joined to Azure, you can the Azure Monitor agent on the MIM server to beginning collecting Windows Event data.  To deploy Azure Monitor you can use the following PowerShell script.  Be sure to replace the variables with your information.  

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

Before we create a data collection rule that will collect the Windows Event log information, we need somewhere to send this information.  Follow the steps outline in [Create a workspace](/azure/azure-monitor/logs/quick-create-workspace?tabs=azure-portal#create-a-workspace) to create a Log Analytics workspace.

## Create a Data Collection Rule
Data collection rules (DCRs) are part of an ETL-like data collection process that improves on legacy data collection methods for Azure Monitor. This process uses a common data ingestion pipeline, the Azure Monitor pipeline, for all data sources and a standard method of configuration that's more manageable and scalable than other methods.

To create the data collection rule for the MIM server.  Use the following steps.

1. On the Monitor home screen in the Azure portal, select **Settings** and **Data Collection Rules**.
2. At the top, click **Create**
3. Give your rule a name, assoicate it with your resource group, and the region your resource group is located in.
4. Click **Next**
5. On the resources tab, click **Add resources** and under your resource group, add the MIM server.  Click **Next**.
6. On **Collect and deliver** and the **Windows Event Logs** as the data source.
7. On **Basic** you can add the basic Windows Event logs, System, Security, and Application.
8. Click on **Custom**.
9. Enter the following in the box under **Use XPath queries to filter event logs and limit data collection**

|Xpath querie|Description|
|-----|-----|
|Forefront Identity Manager!*[System[(Level=1 or Level=2 or Level=3 or Level=4 or Level=0 or Level=5)]]| The MIM service log.
|Forefront Identity Manager Management Agent!*[System[(Level=1 or Level=2 or Level=3 or Level=4 or Level=0 or Level=5)]]|The MIM management agent log|
|Forefront Identity Manager Synchronization%4Operational!*[System[(Level=1 or Level=2 or Level=3 or Level=4 or Level=0 or Level=5)]]|The operations log for the MIM synchronization engine|

:::image type="content" source="media/mim-azure-monitor-reporting/azure-reporting-2.png" alt-text="Screenshot of data collection sources." lightbox="media/mim-azure-monitor-reporting/azure-reporting-2.png":::

10. Click **Next Destination** and click **Add Destination**
11. Enter the following:
    - Destination Type: Azure Monitor Logs
    - Subscriptiong: Your subscription
    - Destination Details: Your workgroup

12. Click **Add data source**
13. Click **Review and Create**
14. Click **Create**

Once this completes, event log information will begin to flow from the MIM server.

## Verify data
To verify that you are collecting data, you can go to your workspace and run the following query.

1. On your workspace, select logs
2. Enter the following query: `Event | where TimeGenerated > ago(48h)`
3. You should see your MIM data.
 
 :::image type="content" source="media/mim-azure-monitor-reporting/azure-reporting-3.png" alt-text="Screenshot of data collected." lightbox="media/mim-azure-monitor-reporting/azure-reporting-3.png":::

