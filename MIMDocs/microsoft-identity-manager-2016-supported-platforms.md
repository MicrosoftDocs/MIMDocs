---
# required metadata

title: Supported software platforms | Microsoft Docs
description: Find the products and versions that are compatible with each of the MIM 2016 components
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 5/14/2021
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer:
ms.suite: ems
#ms.tgt_pltfrm:
ms.custom: mim

---

# Supported platforms for MIM 2016

This table describes the supported platforms and version for each component of Microsoft Identity Manager 2016. The versions marked with a * are only supported in MIM 2016 Service Pack 1, Service Pack 2 or a later hotfix. The versions marked with ** are only supported in MIM 2016 Service Pack 2 or a later hotfix. The versions marked with "NR", for not recommended, are supported but are not recommended if starting a fresh deployment of that platform for MIM.


| **MIM component** | **Platform** | **Version** |
|-------------------|--------------|--------------|
| **MIM Sync** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2<br/>Windows Server 2016 *<br/>Windows Server 2019 ** |
| | Active Directory functional level for user provisioning, PCNS and GAL Sync | Windows 2000 (NR)<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | MIM Sync database | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/>SQL Server 2016 SP2 * <br/>SQL Server 2017 ** <br/> SQL Server 2019 ** |
| | Active Directory for user provisioning, PCNS, and GAL Sync (optional)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Exchange for mailbox provisioning and GAL Sync (optional)|Exchange Server 2013 SP1<br/>Exchange Server 2016 *<br/>Exchange Server 2019 ** |
| | Development environment (optional) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017 * |
| | Additional connected system (optional) | Active Directory Domain Services<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2008 or later<br/>SharePoint Server 2013<br/> SharePoint Server 2016 *<br/> SharePoint Server 2019 ** <br/> Other third-party products |
| **MIM Service and Portal** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| |PAM Scenario: Windows Server | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * <br/> Windows Server 2019 **|
| |PAM Scenario: Active Directory for bastion environment PAM forest | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * <br/> Windows Server 2019 ** |
| |PAM Scenario: Active Directory for PAM scenario existing (CORP) forests | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * <br/> Windows Server 2019 ** |
| | MIM Service database | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 *<br/> SQL Server 2017 ** <br/> SQL Server 2019 ** |
| | SharePoint | SharePoint Foundation 2013 SP1 (NR) <br/> SharePoint 2016 *<br/> SharePoint 2019 ** |
| | Mail server for MIM Service approval and group management emails (optional) | Exchange Server 2013 SP1 <br/> Exchange Server 2016 *<br/> Exchange Server 2019 ** <br/> Exchange Online * (Notification only before build [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490)) |
| | Browser | All major supported browsers * (Mobile devices limited)|
| **MIM Service Reporting** | Windows Server |  Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR) <br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Data warehouse | System Center 2012 Service Manager SP1 <br/> System Center 2012 R2 Service Manager <br/> System Center 2016 Service Manager * (With 4.4.1459)<br/> System Center 2019 Service Manager ** |
| **MIM Password Reset and Registration Portals** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Web browser | All major supported browsers |
| **MIM Add-ins and Extensions** | Windows | Windows 7<br/>Windows 8.1<br/>Windows 10 |
| | Outlook integration (optional) | Outlook 2013 (on Windows, except Click-To-Run) <br/> Outlook 2016 (on Windows 10, except Click-To-Run) *<br/>Outlook for Microsoft 365 (on Windows 10, including Click-To-Run) ** |
| | PAM PowerShell requestor cmdlets (optional) | Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (Server and CA integration) | Windows server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Certificate authority | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | MIM CM database | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 *<br/> SQL Server 2017 ** |
| **MIM Certificate Management** (Application) | Windows | Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (Bulk Client) | Windows | Windows 7 |
| **MIM Certificate Management** (Client ActiveX based smart card) | Windows | Windows 7 <br/>Windows 8.1 <br/> Windows 10 |
| **MIM BHOLD Suite** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | BHOLD database | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4  <br/> SQL Server 2014 SP3 * <br/> SQL Server 2016 SP2 * |
| | Mail server (optional) | Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |

