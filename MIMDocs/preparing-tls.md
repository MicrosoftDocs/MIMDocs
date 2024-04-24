---
# required metadata

title: Planning Microsoft Identity Manager 2016 in TLS 1.2 environment | Microsoft Docs
description: Planning Microsoft Identity Manager 2016 in TLS 1.2 environment
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 09/14/2023
ms.topic: conceptual
ms.service: microsoft-identity-manager

ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Planning MIM 2016 SP2 in TLS 1.2 or FIPS-mode environments


> [!IMPORTANT]
> This article applies to MIM 2016 SP2 only

When installing MIM 2016 SP2 in the locked-down environment that has all encryption protocols but TLS 1.2 disabled, the following requirements apply:
- To establish secure TLS 1.2 connection MIM components require the latest updates for Windows Server and .NET Framework that enable TLS 1.2 support in .NET 3.5 Framework to be installed. Depending on your server configuration, you *might* need to [enable SystemDefaultTlsVersions for .NET Framework](https://support.microsoft.com/help/3154520/support-for-tls-system-default-versions-included-in-the-net-framework) and / or [disable all SCHANNEL protocols except TLS 1.2](/windows-server/security/tls/tls-registry-settings) in registry in *Client* and *Server* subkeys.

## MIM Synchronization Service, SQL MA

- To establish secure TLS 1.2 connection with SQL server, MIM Synchronization Service and built-in SQL management agent require [SQL Native Client 11.0.7001.0](https://www.microsoft.com/download/details.aspx?id=50402) or later.

## MIM Service
   >[!NOTE]
   >MIM 2016 SP2 unattended install fails in TLS 1.2 only environment. Either install MIM Service in interactive mode or, if installing unattended, make sure TLS 1.1 is enabled. After unattended installation completes, enforce TLS 1.2 if needed.

- Self-signed certificates cannot be used by MIM Service in TLS 1.2 only environment. Choose strong encryption compatible certificate issued by trusted Certification Authority when installing MIM Service.
- MIM Service installer additionally requires [OLE DB Driver for SQL Server version 18.2](https://www.microsoft.com/download/details.aspx?id=56730) or later.

## FIPS-mode considerations

If you install MIM Service on a server with FIPS-mode enabled you need to disable FIPS policy validation to allow MIM Service Workflows to be executed. To do so, add *enforceFIPSPolicy enabled=false* element into *runtime* section of *Microsoft.ResourceManagement.Service.exe.config* file between *runtime* and *assemblyBinding* sections as shown below:

```XML
<runtime>
<enforceFIPSPolicy enabled="false"/>
<assemblyBinding ...>
```    
