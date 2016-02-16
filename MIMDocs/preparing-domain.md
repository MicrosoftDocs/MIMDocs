---
title: Preparing the Domain
ms.custom: 
  - Identity Management
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
author: Kgremban
---
# Preparing the Domain

## Update the domain

1.  MIM requires that Active Directory is already installed. Make sure you have a domain controller in your environment for a domain that you are able to administer.

2.  Log on to the domain controller as the domain administrator (*e. g. Contoso\Administrator*) and create the following user accounts for MIM services.

    1.  Start PowerShell.

    2.  Type the following PowerShell script to update the domain.

        > [!NOTE]
        > In all the examples below, **mimservername** represents the name of your domain controller, **domain** represents your domain name and **Pass@word1** represents an example password.

        ```
        import-module activedirectory
        $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
        New-ADUser –SamAccountName MIMMA –name MIMMA 
        Set-ADAccountPassword –identity MIMMA –NewPassword $sp
        Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName MIMSync –name MIMSync 
        Set-ADAccountPassword –identity MIMSync –NewPassword $sp
        Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName MIMService –name MIMService 
        Set-ADAccountPassword –identity MIMService –NewPassword $sp
        Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName MIMSSPR –name MIMSSPR 
        Set-ADAccountPassword –identity MIMSSPR –NewPassword $sp
        Set-ADUser –identity MIMSSPR –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName SharePoint –name SharePoint 
        Set-ADAccountPassword –identity SharePoint –NewPassword $sp
        Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName SqlServer –name SqlServer 
        Set-ADAccountPassword –identity SqlServer –NewPassword $sp
        Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1
        New-ADUser –SamAccountName BackupAdmin –name BackupAdmin 
        Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
        Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1
        ```

3.  Create security groups to all the groups.

    1.  Run the following PowerShell script:

    ```
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global 		–SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global 		–SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global 		–SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global 		–SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global          –SamAccountName MIMSyncPasswordReset 
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    ```

4.  Using PowerShell, add SPNs to enable Kerberos authentication for service accounts

    ```
    setspn -S http/mimservername.domain.local Domain\SharePoint
    setspn -S http/mimservername Domain\SharePoint
    setspn -S FIMService/mimservername.domain.local Domain\MIMService
    setspn -S FIMSync/mimservername.domain.local Domain\MIMSync
    ```

