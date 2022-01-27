---
# required metadata

title: Deploy PAM Step 1 - CORP domain | Microsoft Docs
description: Prepare the CORP domain with existing or new identities to be managed by Microsoft Identity Manager
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---
# Step 1 - Prepare the host and the CORP domain

> [!div class="step-by-step"]
> [Step 2 »](step-2-prepare-priv-domain-controller.md)

In this step, you'll prepare to host the environment that will be managed by PAM. If necessary, you'll also create a domain controller and a member workstation in a new domain and forest (the *CORP* forest). Access to that forest will be from identities to be managed by the bastion environment, with a *PRIV* forest, created in the next step. This CORP forest simulates an existing forest that has resources to be managed. This document includes an example resource to be protected, a file share.

If you already have an existing Active Directory (AD) domain with a domain controller running Windows Server 2012 R2 or later, where you are a domain administrator, you can use that domain instead, and skip down to the section "Create a group" in this article.

## Prepare the CORP domain controller

This section describes how to set up a domain controller for a CORP domain. In the CORP domain, the administrative users are managed by the bastion environment. The Domain Name System (DNS) name of the CORP domain used in this example is *contoso.local*.

### Install Windows Server

Install Windows Server 2016 or later on a virtual machine to create a computer called *CORPDC*.

1. Choose **Windows Server 2016 (Server with Desktop Experience)**.

2. Review and accept the license terms.

3. Since the disk will be empty, select **Custom: Install Windows only** and use the uninitialized disk space.

4. Sign in to that new computer as its administrator. Navigate to the Control Panel. Set the computer name to *CORPDC*, and give it a static IP address on the virtual network. Restart the server.

5. After the server has restarted, sign in as an administrator. Navigate to the Control Panel. Configure the computer to check for updates, and install any updates needed. Restart the server.

### Add roles to establish a domain controller

In this section, you'll set up the new Windows Server to become a domain controller.  You'll add the Active Directory Domain Services (AD DS), DNS Server, and File Server (part of the File and Storage Services section) roles, and promote this server to a domain controller of a new forest contoso.local.

> [!NOTE]  
> If you already have a domain to use as your CORP domain, and that domain uses Windows Server 2012 R2 or later as its domain functional level, you can skip to [Create additional users and groups for demonstration purposes](#create-additional-users-and-groups-for-demonstration-purposes).

1. While signed in as an administrator, launch PowerShell.

2. Type the following commands.

   ```PowoerShell
   import-module ServerManager

   Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

   Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
   ```

   This will prompt for a Safe Mode Administrator Password to use. Note that warning messages for DNS delegation and cryptography settings will appear. These are normal.

3. After the forest creation is complete, sign out. The server will restart automatically.

4. After the server restarts, sign in to CORPDC as an administrator of the domain. This is typically the user CONTOSO\\Administrator, which will have the password that was created when you installed Windows on CORPDC.

### Install updates (Windows Server 2012 R2 only)

1. If you choose to use Windows Server 2012 R2 as the operating system for CORPDC, you must install hotfixes 2919442, 2919355, [and update 3155495](https://support.microsoft.com/kb/3156418) on CORPDC.

### Create a group

Create a group for auditing purposes by Active Directory, if the group does not already exist. The name of the group must be the NetBIOS domain name followed by three dollar signs, for example *CONTOSO$$$*.

For each domain, sign in to a domain controller as a domain administrator, and perform the following steps:

1. Launch PowerShell.

2. Type the following commands, but replace "CONTOSO" with the NetBIOS name of your domain.

   ```PowerShell
   import-module activedirectory

   New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
   ```

In some cases the group may already exist - this is normal if the domain was also used in AD migration scenarios.

### Create additional users and groups for demonstration purposes

If you created a new CORP domain, then you should create additional users and groups for demonstrating the PAM scenario. The user and group for demonstration purposes should not be domain administrators or controlled by the adminSDHolder settings in AD.

> [!NOTE]
> If you already have a domain you'll be using as the CORP domain, and it has a user and a group that you can use for demonstration purposes, then you can skip to the section [Configure auditing](#configure-auditing).

We're going to create a security group named *CorpAdmins* and a user named *Jen*. You can use different names if you wish.  If you already have an existing user, e.g., with a smartcard, then you won't need to create a new user.

1. Launch PowerShell.

2. Type the following commands. Replace the password 'Pass@word1' with a different password string.

   ```PowerShell
   import-module activedirectory

   New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

   New-ADUser –SamAccountName Jen –name Jen

   Add-ADGroupMember –identity CorpAdmins –Members Jen

   $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

   Set-ADAccountPassword –identity Jen –NewPassword $jp

   Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
   ```

### Configure auditing

You need to enable auditing in existing forests in order to establish the PAM configuration on those forests.  

For each domain, sign in to a domain controller as a domain administrator, and perform the following steps:

1. Go to **Start** >  **Windows Administrative Tools**, and launch **Group Policy Management**.

2. Navigate to the domain controllers policy for this domain.  If you created a new domain for contoso.local, navigate to **Forest: contoso.local** > **Domains** > **contoso.local** > **Domain Controllers** > **Default Domain Controllers Policy**. An informational message appears.

3. Right-click on **Default Domain Controllers Policy** and select **Edit**. A new window appears.

4. In the Group Policy Management Editor window, under the Default Domain Controllers Policy tree, navigate to **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Local Policies** > **Audit Policy**.

5. In the details pane, right click on **Audit account management** and select **Properties**. Select **Define these policy settings**, put a checkbox on **Success**, put a checkbox on **Failure**, click **Apply** and **OK**.

6. In the details pane, right click on **Audit directory service access** and select **Properties**. Select **Define these policy settings**, put a checkbox on **Success**, put a checkbox on **Failure**, click **Apply** and **OK**.

7. Close the Group Policy Management Editor window and the Group Policy Management window.

8. Apply the audit settings by launching a PowerShell window and typing:

   ```cmd
   gpupdate /force /target:computer
   ```

The message **Computer Policy update has completed successfully** should appear after a few minutes.

### Configure registry settings

In this section you'll configure the registry settings that are needed for sID History migration, which will be used for Privileged Access Management group creation.

1. Launch PowerShell.

2. Type the following commands to configure the source domain to permit remote procedure call (RPC) access to the security accounts manager (SAM) database.

   ```PowerShell
   New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

   Restart-Computer
   ```

This will restart the domain controller, CORPDC. For further information on this registry setting, see [How to troubleshoot inter-forest sIDHistory migration with ADMTv2](https://support.microsoft.com/kb/322970).

## Prepare a CORP resource for demonstration purposes

You'll need at least one resource in the domain for demonstrating the security group-based access control with PAM.  If you don't already have a resource, you can use a file folder on a server joined to the *CORP* domain for the purposes of demonstration.  This will make use of the "Jen" and "CorpAdmins" AD objects you created in the contoso.local domain.

1. Connect to the server as an administrator.

2. Create a new folder named *CorpFS* and share it with the *CorpAdmins* group.  Open PowerShell as an administrator, and type the following commands.

   ```PowerShell
   mkdir c:\corpfs

   New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

   $acl = Get-Acl c:\corpfs

   $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

   $acl.SetAccessRule($car)

   Set-Acl c:\corpfs $acl
   ```

In the next step, you'll prepare the PRIV domain controller.

> [!div class="step-by-step"]
> [Step 2 »](step-2-prepare-priv-domain-controller.md)
