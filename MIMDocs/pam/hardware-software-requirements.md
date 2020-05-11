---
# required metadata

title: PAM software requirements | Microsoft Docs
description: Find the hardware and software requirements for a successful deployment of Privileged Access Management
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/06/2017
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---
# Hardware and software requirements

Privileged Access Management has no hardware requirements beyond those of the underlying software platforms. Just make sure you have sufficient memory or disk space, and network connectivity.

> [!IMPORTANT]
> This article provides the minimum requirements for a basic deployment. It is not intended to demonstrate performance, scalability, or high availability. It does not represent a recommended deployment topology for large enterprises or production environments.

## Installing from software packages

The following software can be downloaded from TechNet Evaluation Center or MSDN:

- Microsoft Identity Manager 2016
  - Service and Portal: contains the installer for MIM Service and MIM Portal and for the PAM Scenario
  - Add-ins and Extensions: contains the installer for the requestor PowerShell cmdlets

The following software can be downloaded from GitHub:

- [PAMSamplePortal](https://github.com/Azure/identity-management-samples): contains sample web application for the REST API

## Required software

- Windows Server 2012 R2
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 or SQL Server 2014

## Evaluation software

If you do not have licenses for Windows, SQL Server, or Windows Server you can download evaluation versions.

### TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)

### Microsoft Download Center

- [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)  
- [SharePoint Foundation 2013 SP1 and its prerequisites](https://www.microsoft.com/download/details.aspx?id=42039)

## Hardware requirements

For each component of PAM, refer to the system requirements of the software products.

For CORPDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) or earlier

For CORPWKSTN:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

For PRIVDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

For PAMSRV:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) or [SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
