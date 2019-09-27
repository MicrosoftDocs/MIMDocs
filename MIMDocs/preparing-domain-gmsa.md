---
# required metadata

title: Set up a domain for Microsoft Identity Manager 2016 | Microsoft Docs
description: Create an Active Directory domain controller before installing MIM 2016
keywords:
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/26/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager

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

# Set up a domain for Group Managed Service Accounts (gMSA) scenario

> [!div class="step-by-step"]
> [Windows Server »](prepare-server-ws2016.md)

> [!IMPORTANT]
> This article applies to MIM 2016 SP2 only

Microsoft Identity Manger (MIM) works with your Active Directory (AD) domain. You should already have AD installed, and make sure you have a domain controller in your environment for a domain that you are able to administer.

# Overview

Group Managed Service Accounts eliminate the need to periodically change service accounts passwords. With the release of MIM 2016 SP2 the following MIM components support gMSA accounts to be used during the installation process: 

-   MIM Synchronization service (FIMSynchronizationService)
-   MIM Service (FIMService)
-   MIM Service Management Agent 
-   MIM Password Registration Web Site 
-   MIM Password Reset Web Site
-   PAM Monitoring Service (PamMonitoringService)
-   PAM Component Service (PrivilegeManagementComponentService)

The following MIM components do not support gMSA accounts:

-   MIM Portal is not supported it is part of the SharePoint environment and you would need to deploy in farm mode and [Configure automatic password change in SharePoint Server](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   All Management Agents except MIM Service
-   Microsoft Certificate Management
-   BHOLD

As a general rule, in most cases when you want a MIM component installer to use gMSA instead of a regular account, you append the dollar sign to gMSA name, e.g. **contoso\MIMSyncGMSAsvc$**, and leave password empty. The only known exception from this rule is *miisactive.exe* tool that accepts gMSA name without the dollar sign.
<br/>

More information about gMSA can be found in these articles:
-   [Group Managed Service Accounts Overview](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   [New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)

-   [Create the Key Distribution Services KDS Root Key](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx)



## Create user accounts and groups

All the components of your MIM deployment need their own identities in the domain. This includes the MIM components like Service and Sync, as well as SharePoint and SQL.

> [!NOTE]
> This walkthrough uses sample names and values from a company called Contoso. Replace these with your own. For example:
> - Domain controller name - **dc**
> - Domain name - **contoso**
> - MIM Service Server name - **mimservice**
> - MIM Sync Server name - **mimsync**
> - SQL Server name - **sql**
> - Password - <strong>Pass@word1</strong>

1. Sign in to the domain controller as the domain administrator (*e. g. Contoso\Administrator*).

2. Create the following user accounts for MIM services. Start PowerShell and type the following PowerShell script to create new AD domain users (not all accounts are mandatory, although the script is provided for informational purposes only, it is a best practice to use a dedicated *MIMAdmin* account for MIM and SharePoint install process).

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

    New-ADUser –SamAccountName MIMAdmin –name MIMAdmin
    Set-ADAccountPassword –identity MIMAdmin –NewPassword $sp
    Set-ADUser –identity MIMAdmin –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcSharePoint –name svcSharePoint
    Set-ADAccountPassword –identity svcSharePoint –NewPassword $sp
    Set-ADUser –identity svcSharePoint –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMSql –name svcMIMSql
    Set-ADAccountPassword –identity svcMIMSql –NewPassword $sp
    Set-ADUser –identity svcMIMSql –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName BackupAdmin –name BackupAdmin
    Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
    Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMAppPool –name svcMIMAppPool
    Set-ADAccountPassword –identity svcMIMAppPool –NewPassword $sp
    Set-ADUser –identity svcMIMAppPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Create security groups to all the groups.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupMember -identity MIMSyncAdmins -Members MIMAdmin
    ```

4.  Add SPNs to enable Kerberos authentication for service accounts

    ```PowerShell
    Set-ADServiceAccount -Identity svcMIMAppPool -ServicePrincipalNames @{Add="http/mim.contoso.com,http/mim"}
    ```

5.  Make sure to register the following DNS 'A' records for proper name resolution (assuming that MIM Service, MIM Portal, Password Reset and Password Registration web sites will be hosted on the same machine)

    - mim.contoso.com - point to MIM Service and Portal server physical ip address
    - passwordreset.contoso.com - point to MIM Service and Portal server physical ip address
    - passwordregistration.contoso.com - point to MIM Service and Portal server physical ip address

## Create Key Distribution Service Root Key
First Step on your windows domain controller

1.  Create the Key Distribution Services (KDS) Root Key (only once per domain) if
    needed. Root Key is used by the KDS service on DCs (along with other
    information) to generate passwords. Start PowerShell and type the following PowerShell script as domain admin:

    ```PowerShell
    Add-KDSRootKey –EffectiveImmediately
    ```
    *–EffectiveImmediately* means wait up to \~10 hours / need to replicate
        to all DC. This was approximately 1 hour for two domain controllers.

    ![](media/7fbdf01a847ea0e330feeaf062e30668.png)

    >[!NOTE]
In the Lab or Test environment you can skip 10 hours replication delay by running the following command instead:<br/>
Add-KDSRootKey -EffectiveTime ((Get-Date).AddHours(-10))

## Create MIM Synchronization Service accounts and groups
-----------------------

1.  Create a group called *MIMSync_Servers* and add all MIM Synchronization servers to this group.
    Start PowerShell and type the following PowerShell script as domain admin to create new AD group for MIM Synchronization Servers and add MIM servers, e.g. *MIMSync.contoso.com*, into this group.

    ```PowerShell
    New-ADGroup –name MIMSync_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMSync_Servers
    Add-ADGroupmember -identity MIMSync_Servers -Members MIMSync$
    ```

2.  Create MIM Synchronization Service gMSA. Start PowerShell and type the following PowerShell as domain admin with
    computer account already joined to the domain

    ```PowerShell
    New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"
    ```

    Check details of the GSMA created by executing *Get-ADServiceAccount* PowerShell command:

    ![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

3.  If you plan to run Password Change Notification Service, you need to register Service Principal Name by executing this PowerShell command:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames @{Add="PCNSCLNT/mimsync.contoso.com,PCNSCLNT/mimsync"}
    ```

    >[!IMPORTANT]
 You can skip creation of the MIM Service Management Agent service account (MIM MA account) and enable 'Use MIMSync Account' option when creating MIM Service Management Agent. In this case, use MIM Synchronization Service gMSA name, e.g. contoso\MIMSyncGMSAsvc$, instead of the MIM MA account in the MIM Service installer and leave the password empty. <br/>Also, do not enable 'Deny Logon from Network' for the MIM Synchronization Service gMSA as MIM MA account requires 'Allow Network Logon' permissions.

## Create MIM Service accounts and groups

1.  Create a group called *MIMService_Servers* and add all MIM Service and Portal servers to this group.
    Start PowerShell and type the following PowerShell script as domain admin to create new AD group for MIM Service and Portal Servers and add MIM servers, e.g. *MIMPortal.contoso.com*, into this group.

    ```PowerShell
    New-ADGroup –name MIMService_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMService_Servers
    Add-ADGroupMember -identity MIMService_Servers -Members MIMPortal$
    ```

2.  Create MIM Service gMSA. Start PowerShell and type the following PowerShell as domain admin with
    computer account already joined to the domain

    ```PowerShell
    New-ADServiceAccount -Name MIMSrvGMSAsvc -DNSHostName MIMSrvGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMService_Servers"
    ```

3.  Register Service Principal Name and enable Kerberos delegation by executing this PowerShell command:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSrvGMSAsvc -ServicePrincipalNames @{Add="FIMService/mimportal.contoso.com,FIMService/mimportal,FIMService/mim.contoso.com,FIMService/mim"}
    Set-ADServiceAccount -Identity MIMSrvGMSAsvc -TrustedForDelegation $true -OtherAttributes @{'msDS-AllowedToDelegateTo'='FIMService/mimportal.contoso.com,FIMService/mimportal,FIMService/mim.contoso.com,FIMService/mim'}
    ```

4.  For SSPR scenarios you need MIM Service Account be able to communicate with MIM Synchronization Service, threfor MIM Service account must be either a member of MIMSyncAdministrators or MIM Sync Password Reset and Browse groups:
    ```PowerShell
    Add-ADGroupmember -identity MIMSyncPasswordReset -Members svcMIMService 
    Add-ADGroupmember -identity MIMSyncBrowse -Members svcMIMService 
    ```

## Other MIM accounts and groups
Following the same principals as describe above for MIM Synchronization Service and MIM Service you can create other gMSA  for:
- MIM Password Reset web site application pool
- MIM Password Registration web site application pool
- MIM PAM REST API web site application pool
- MIM PAM Component service
- MIM PAM Monitoring service

> [!div class="step-by-step"]
> [Windows Server »](prepare-server-ws2016.md)
