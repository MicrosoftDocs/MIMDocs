---
# required metadata

title: Hardware and software requirements | Microsoft Identity Manager
description:
keywords:
author: kgremban
manager: stevenpo
ms.date: 06/10/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid:

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

No hardware requirements beyond those of the underlying software platforms, sufficient memory or disk space and network connectivity is required. This test lab is not intended to demonstrate performance, scalability or high availability, and does not represent a recommended deployment topology for large enterprises or production environments.

## Installing from software packages

The following software can be downloaded from TechNet Evaluation Center or MSDN:  
- Microsoft Identity Manager 2016
  - Service and Portal: contains the installer for MIM Service and MIM Portal and for the PAM Scenario
  - Add-ins and Extensions: contains the installer for the requestor PowerShell cmdlets

The following software can be downloaded from GitHub:  
- PAMSamplePortal: contains sample web application for the REST API

## Required software

- Windows Server 2012 R2  
- Windows 8.1 Enterprise or Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 or SQL Server 2014  

## Evaluation software

If you do not have licenses for Windows, SQL Server, or Windows Server you can download evaluation versions.

### TechNet Evaluation Center

[Download Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)
[Download Windows 8.1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)
[Download Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)

### Microsoft Download Center

[Download SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)
[Download SharePoint Foundation 2013 SP! and its prerequisites](https://www.microsoft.com/download/details.aspx?id=42039)

## Hardware requirements

For each component of PAM, refer to the system requirements of the software products.

For CORPDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) or earlier

For CORPWKSTN:  
- [Windows 8.1](http://windows.microsoft.com/windows-8/system-requirements)

For PRIVDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

For PAMSRV:
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) or [SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)
