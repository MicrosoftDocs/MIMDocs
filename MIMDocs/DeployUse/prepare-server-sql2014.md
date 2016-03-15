---
title: Preparing an identity management server&#58; SQL Server 2014 | Microsoft Identity Manager
ms.custom:
  - Identity Management
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - security
ms.tgt_pltfrm: na
ms.topic: get-started-article
author: kgremban
---
# Preparing an identity management server: SQL Server 2014

>[!div class="step-by-step"]
[Previous](https://docsmsftstage.azurewebsites.net/MIM/DeployUse/prepare-server-ws2012r2.html)
**Preparing an identity management server: Windows Server 2012 R2**

> [!NOTE]
> In all the examples below, **mimservername** represents the name of your domain controller, **contoso** represents your domain name, and **Pass@word1** represents an example password.

## Install **SQL Server 2014 Standard Edition**

1. Launch **PowerShell** as a domain administrator.

2. Change to the directory where the SQL Server setup program is located.

3. Type the following commands.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[Next](https://docsmsftstage.azurewebsites.net/MIM/DeployUse/prepare-server-sharepoint.html)
**Preparing an identity management server: SharePoint**
