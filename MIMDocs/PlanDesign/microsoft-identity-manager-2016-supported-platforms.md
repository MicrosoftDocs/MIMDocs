---
# required metadata

title: Supported software platforms | Microsoft Identity Manager
description: Find the products and versions that are compatible with each of the MIM 2016 components
keywords:
author: kgremban
manager: femila
ms.date: 09/29/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Supported platforms for MIM 2016

This table describes the supported platforms and version for each component of Microsoft Identity Manager 2016. The versions marked with a * are only supported in MIM 2016 service pack 1.

| **MIM component** | **Platform** | **Version** |
|-------------------|--------------|-------------|
|**MIM Sync**|Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | MIM Sync database |SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Active Directory for user provisioning, PCNS and GAL Sync (optional)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Exchange for mailbox provisioning and GAL Sync (optional)|Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
| | Development environment (optional) | Visual Studio 2012<br/>Visual Studio 2013 |
| | Additional connected system (optional) | Active Directory Domain Services<br/>Active Directory<br/>Lightweight Directory Services<br/>SQL Server 2000 or later<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Other third party products |
| **MIM Service** (except PAM scenario) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | MIM Service database | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Exchange for MIM Service approval and group management emails (optional) | Exchange Server 2007 SP3 (with installed Exchange management console)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * |
| **MIM Service and Portal** (PAM scenario only)| Windows Server | Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Active Directory for bastion environment PAM forest | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Active Directory for existing forests | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | MIM Service database | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | Mail server for MIM Service approval and group management emails (optional) | Exchange Server 2007 SP3 (with installed Exchange management console)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * |
| | Browser | All major browsers |
| **MIM Service Reporting** | Windows Server | Windows Server 2012 <br/> Windows Server 2016 * |
| | Data warehouse | System Center 2012 Service Manager SP1 |
| | Data warehouse database | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 |
| **MIM Password Reset and Registration Portals** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Web browser | All major browsers |
| **MIM Add-ins and Extensions** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Outlook integration (optional) | Outlook 2007 SP2<br/>Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (on Windows 10) * |
| | PAM PowerShell requestor cmdlets (optional) | Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (Server and CA integration) | Windows server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 |
| | Certificate authority | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 |
| | MIM CM database | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 |
| **MIM Certificate Management** (Application) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (Client and Bulk Client) | Windows | Windows 7 |
| **MIM BHOLD Suite** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 |
| | BHOLD database | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * |
| | Mail server (optional) | Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
| | Web browser | Internet Explorer 7, 8, 9, 10 or 11 with Silverlight |
