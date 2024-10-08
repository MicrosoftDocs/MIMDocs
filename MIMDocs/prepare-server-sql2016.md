---
# required metadata

title: Configure SQL Server for Microsoft Identity Manager 2016 SP2 | Microsoft Docs
description: Install SQL Server 2016 or 2017 in preparation for your MIM 2016 installation.
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 09/14/2023
ms.topic: conceptual
ms.service: microsoft-identity-manager

ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Set up an identity management server: SQL Server 2016 or 2017

> [!div class="step-by-step"]
> [« Windows Server](prepare-server-ws2016.md)
> [SharePoint »](prepare-server-sharepoint.md)
 
> [!NOTE] 
> Sql Server 2017 setup procedure does not differ from Sql Server 2016 setup procedure.

> [!NOTE]
> This walkthrough uses sample names and values from a company called Contoso. Replace these with your own. For example:
> - Domain controller name - **corpdc**
> - Domain name - **contoso**
> - MIM Service Server name - **corpservice**
> - MIM Sync Server name - **corpsync**
> - SQL Server name - **corpsql**
> - Password - <strong>Replace with your own strong password</strong>

> [!IMPORTANT]
> MIM 2016 SP2 supports SQL AlwaysOn Availability Group (AoAG) listeners with *RegisterAllProvidersIP* option set to 0, meaning that SQL Server AoAG cross-subnet failover is not currently supported.

> [!IMPORTANT]
> [SQL Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) is supported by MIM Synchronization Service and MIM Service and Portal components with MIM SP2 or later builds.

## Install **SQL Server 2016 Standard/Enterprise Edition**

1. Launch **PowerShell** as a domain administrator.

2. Change to the directory where the SQL Server setup program is located.

3. Type the following commands.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="<Replace with your own strong password>"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```
    
More info SQL deployment accounts and services can be found [here](/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)

> [!NOTE]
> SSMS is no longer included in SQL 2016. Download details can be found [here](/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)

> [!div class="step-by-step"]  
> [« Windows Server](prepare-server-ws2016.md)
> [SharePoint »](prepare-server-sharepoint.md)
