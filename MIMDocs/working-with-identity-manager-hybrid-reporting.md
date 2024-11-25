---
# required metadata

title: Work with hybrid reporting in Microsoft Entra by using Identity Manager 2016 
description: Learn how to combine on-premises and cloud data into hybrid reports in Microsoft Entra, and how to manage and view these reports.
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 11/18/2024
ms.topic: article
ms.service: microsoft-identity-manager

ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Work with hybrid reporting in Identity Manager

This article discusses how to combine on-premises and cloud data into hybrid reports in Microsoft Entra, and how to manage and view these reports.

> [!IMPORTANT]
> The MIM hybrid reporting feature, described in this article, is deprecated. This is replaced by using Azure Arc agent to send event logs to Azure Monitor, as this allows more flexible reports.  As of November 2025, the cloud endpoints used by the MIM hybrid reporting agent will no longer be available, and customers should transition to Azure Monitor or similar.
> For more information, see [Microsoft Identity Manager 2016 reporting with Azure Monitor](mim-azure-monitor-reporting.md).

## Available hybrid reports

The three Microsoft Identity Manager reports available in Microsoft Entra ID are as follows:

- **Password reset activity**: Displays each instance when a user performed password reset using self-service password reset (SSPR) and provides the gates or methods used for authentication.

- **Password reset registration**: Displays each time that a user registers for SSPR and the methods used to authenticate. Examples of methods might be a mobile phone number or questions and answers.
   > [!NOTE]
   > For *Password reset registration* reports, no differentiation is made between the SMS gate and MFA gate. Both are considered mobile phone methods.

- **Self-service groups activity**: Displays each attempt made by someone to add or delete themselves from a group and group creation.

    ![Azure hybrid reporting - password reset activity image](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * The reports currently present data for up to one month of activity.


## Prerequisites

* Identity Manager 2016 SP1 Identity Manager service, Recommended build [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* A Microsoft Entra ID P1 or P2 tenant with a licensed administrator in your directory.

* Outgoing internet connectivity from the Identity Manager server to Azure.

## Requirements
The requirements for using Identity Manager hybrid reporting are listed in the following table:


|                                         Requirement                                         |                                                                                                                                                                                                                                                                                    Description                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Microsoft Entra ID P1 or P2                                       |                                                                                                        Hybrid reporting is a Microsoft Entra ID P1 or P2 feature and requires Microsoft Entra ID P1 or P2. </br>For more information, see [Getting started with Microsoft Entra ID P1 or P2](/azure/active-directory/active-directory-get-started-premium). </br>Get a [free 30-day trial of Microsoft Entra ID P1 or P2](https://azure.microsoft.com/trial/get-started-active-directory/).                                                                                                         |
|                     You must be a global administrator of your Microsoft Entra ID                     |                                                                   By default, only global administrators can install and configure the agents to get started, access the portal, and perform any operations within Azure. </br>**Important**: The account that you use when you install the agents must be a work or school account. It cannot be a Microsoft account. For more information, see [Sign up for Azure as an organization](/azure/active-directory/sign-up-organization).                                                                   |
| Identity Manager Hybrid Agent is installed on each targeted Identity Manager Service server |                                                                                                                                                                                                       To receive the data and provide monitoring and analytics capabilities, hybrid reporting requires the agents to be installed and configured on targeted servers.  </br>                                                                                                                                                                                                       |
|                    Outbound connectivity to the Azure service endpoints                     | During installation and runtime, the agent requires connectivity to Azure service endpoints. If outbound connectivity is blocked by firewalls, ensure that the following endpoints are added to the allowed list:<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net - Port: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li>`https://management.azure.com` </li><li>`https://policykeyservice.dc.ad.msft.net/`</li><li>`https://login.windows.net`</li><li>`https://login.microsoftonline.com`</li><li>`https://secure.aadcdn.microsoftonline-p.com`</li></ul> |
|                         Outbound connectivity based on IP addresses                         |                                                                                                                                                                                                                      For IP addresses based filtering on firewalls, refer to the [Azure IP Ranges](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 SSL inspection for outbound traffic is filtered or disabled                 |                                                                                                                                                                                                               The agent registration step or data upload operations might fail if there is SSL inspection or termination for outbound traffic at the network layer.                                                                                                                                                                                                                |
|                      Firewall ports on the server that runs the agent                       |                                                                                                                                                                                                          To communicate with the Azure service endpoints, the agent requires the following firewall ports to be open:<ul><li>TCP port 443</li><li>TCP port 5671</li></ul>                                                                                                                                                                                                          |
|          Allow certain websites if Internet Explorer enhanced security is enabled           |                                                                                If Internet Explorer enhanced security is enabled, the following websites must be allowed on the server that has the agent installed:<ul><li><https://login.microsoftonline.com></li><li>`https://secure.aadcdn.microsoftonline-p.com`</li><li><https://login.windows.net></li><li>The federation server for your organization trusted by Microsoft Entra ID (for example, `https://sts.contoso.com`).</li></ul>                                                                                |

</BR>

<a name='install-identity-manager-reporting-agent-in-azure-ad'></a>

## Install Identity Manager Reporting Agent in Microsoft Entra ID

After Reporting Agent is installed, the data from Identity Manager activity is exported from Identity Manager to Windows Event Log. Identity Manager Reporting Agent processes the events and then uploads them to Microsoft Entra. In Microsoft Entra, the events are parsed, decrypted, and filtered for the required reports.

1.  Before reinstalling, the previous Hybrid Reporting Agent must be uninstalled. To uninstall hybrid reports, uninstall the MIMreportingAgent.msi agent.

2.  Download Identity Manager Reporting Agent, and then do the following:

    a. Sign in to the Microsoft Entra management portal, and then select **Active Directory**.

    b. Double-click the directory for which you are a global administrator and have a Microsoft Entra ID P1 or P2 subscription.

    c. Select **Configuration**, and then download Reporting Agent.

3.  Install Reporting Agent by doing the following:

    a.  Download the [MIMHReportingAgentSetup.exe file](https://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) for the Identity Manager Service server.

    b.  Run `MIMHReportingAgentSetup.exe`. 

    c.  Run the agent installer.

    d.  Make sure that the Identity Manager Reporting Agent service is running.

    e.  Restart the Identity Manager service.

4.  Verify that Identity Manager Reporting Agent is working in Azure.

    You can create report data by using the Identity Manager self-service password reset portal to reset a user’s password. Make sure that the password reset was completed successfully, and then check to ensure that the data is displayed in the Microsoft Entra management portal.

## View hybrid reports in the Azure portal

1.  Sign in to the [Azure portal](https://portal.azure.com/) with your global administrator account for the tenant.

2.  Select **Microsoft Entra ID**.

3.  In the list of available directories for your subscription, select the tenant directory.

4.  Select **Audit Logs**.

5.  In the **Category** drop-down list, make sure that **MIM Service** is selected.

> [!IMPORTANT]
> It can take some time for Identity Manager audit data to appear in the Azure portal.

## Stop creating hybrid reports

If you want to stop uploading reporting audit data from Identity Manager to Microsoft Entra ID, uninstall Hybrid Reporting Agent. Use the Windows Add or Remove Programs tool to uninstall Identity Manager hybrid reporting.

