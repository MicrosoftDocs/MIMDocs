---
# required metadata

title: Microsoft Identity Manager 2016 | Microsoft Identity Manager
description: Understand how MIM 2016 works to create a safer, more convenient identity management experience in the cloud and on-premises.
keywords:
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/28/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---
# What’s new for Microsoft Identity Manager 2016 Service Pack 1 #

As part of the regular release cycle for servicing and updating Microsoft Identity Manager, we’re pleased to announce [Microsoft Identity Manager (MIM) 2016 Service Pack 1 (SP1)](https://msdn.microsoft.com/subscriptions/downloads/?fileid=70212#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70212). This document outlines the updates, enhancements, features, and changes included in this release.

If you encounter issues during a production deployment of MIM SP1, please contact Microsoft customer support.

We want to hear from you as well! If you have any feedback, comments, or concerns for the product team, email us at [mim2016@microsoft.com.](mailto:mim2016@microsoft.com)



## Updates in this service pack #

### MIM

- **MIM Portal cross-browser compatibility for end-user self-service:** In this Service Pack we are introducing support for most major browsers. Users may now access and interact with the MIM Portal for self-service group and profile management from Edge, Chrome, and Safari.

- **MIM Service support for Exchange Online:** The MIM Service has long supported sending and receiving emails for approvals and notifications. Prior to SP1 MIM only supported Exchange Server or SMTP. With service pack 1, the MIM Service can send and receive requests as well as email notifications using an Office365 Exchange online account.

- **Image file format validation on upload:** MIM is now able to validate the file format of images when they are uploaded to the portal.

### Privileged Access Management(PAM)

- **PAM "PRIV" (bastion) forest support for Windows Server 2016 functional level:** The MIM PAM Service may be configured in an environment with domain controllers running at the Active Directory Domain Services forest functional level of Windows Server 2016. When configured, a user’s Kerberos ticket will be time-limited to the remaining time of their role activation.

    >[!Note]
    If you choose to maintain the forest functional level of Windows Server 2012 R2 in your CORP domain, it is recommended to install [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) and [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) on the CORP domain controller.

- **Privileged account elevation into groups exclusive to the “PRIV” (bastion) forest:** Now, administrators can inform the MIM Service of groups and users exclusive to the “PRIV” forest. Doing this allows these groups and users to be included in PAM roles.  They can then be activated for a role and assigned membership to groups in the “PRIV” forest.

- **PAM Deployment Scripts:** PAM Deployment Scripts allow administrators to streamline the installation of the PAM environment.

- **PAM Cmdlets for Authentication Policy Silo configuration:** Service pack 1 introduces new Cmdlets to harden the security of your bastion forest. These Cmdlets automatically create an Authentication Policy Silo, bound to an Authentication Policy Template.

    >[!Note]
    These Cmdlets run automatically as part of the deployments scripts.


## Platform Support
Updated platform support information may be found in the document called [Supported platforms for MIM 2016](/microsoft-identity-manager/plan-design/microsoft-identity-manager-2016-supported-platforms).  New platforms supported in this service pack include SQL Server 2016, SharePoint 2016

## Issues fixed in this release from MIM 2016 General Availability

### PAM
- New-PAMGroup did not create MIM objects for domain local groups in the PRIV forest
- New-PAMDomainConfiguration would fail with a “netdom” error message
- PAM Monitoring Service logged warnings for groups in the PRIV forest

## How to upgrade to Service Pack 1

Customers upgrading to Microsoft Identity Manager 2016 Service Pack 1 should follow the below guidance on all services applicable to their deployment.

>[!Note]
>Customers running Forefront Identity Manager 2010 R2 SP1 or earlier must first upgrade their environment to Microsoft Identity Manager 2016 released in August of 2015, then follow the steps below.

Before you begin

You need to upgrade the MIM Sync engine prior to upgrading the MIM service and portal.
You need to backup the MIMService and MIM Sync databases.

  1. Uninstall the Microsoft Identity Manager component you are upgrading
  2. Once the uninstall completes, open the splash page located on your installation media “FIMSplash.htm”
  3. Select the MIM component to upgrade
  4. Proceed with the installation following the prompts
    * MIM Service & Portal Installation: When choosing the Exchange Online as the mail account, enter the email address and credentials of the Exchange Online account on the next screen.
