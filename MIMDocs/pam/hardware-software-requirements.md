---
# required metadata

title: PAM software requirements | Microsoft Docs
description: Find the hardware and software requirements for a successful deployment of Privileged Access Management
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/09/2021
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

Privileged Access Management has no hardware requirements beyond those requirements of the underlying software platforms. Just make sure you have sufficient memory or disk space, and network connectivity.

> [!IMPORTANT]
> This article provides the minimum requirements for a basic deployment on an isolated network. It is not intended to demonstrate performance, scalability, or high availability, and does not represent a recommended deployment topology for large enterprises or production environments.  If your Active Directory is part of an Internet-connected environment, see instead the [securing privileged access](/security/compass/overview) guidance for more information on where to start.

## Installing from software packages

The following software can be downloaded from TechNet Evaluation Center or MSDN:

- Microsoft Identity Manager 2016
  - Service and Portal: contains the installer for MIM Service and MIM Portal and for the PAM Scenario
  - Add-ins and Extensions: contains the installer for the requestor PowerShell cmdlets

The following optional software can be downloaded from GitHub:

- [PAMSamplePortal](https://github.com/Azure/identity-management-samples): contains sample web application for the REST API

## Required software

- Windows Server 2016
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 or SQL Server 2014

## Hardware requirements

For each component of PAM, refer to the system requirements of the software products.

For CORPDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) or earlier

For CORPWKSTN:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

For PRIVDC:

- Windows Server 2016

For PAMSRV:

- Windows Server 2016
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) or [SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
