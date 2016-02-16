---
title: Step 1 - Prepare the CORP domain
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
author: Kgremban
---
# Step 1 - Prepare the CORP domain
In this step, you will create a domain controller and a member workstation in a new domain in a new forest. This forest will simulate an existing forest that has resources to be managed. The resource to be protected for this scenario will be a file share.

## Prepare four virtual machines

Prepare four virtual machines with separate drives that are connected to each other on a shared virtual network. These virtual machines can be hosted by Windows 8.1, Windows Server 2012 R2 or other operating system platforms. The drive where the virtual machine disk images will be stored must have a minimum of 100 GB of free disk space to hold all the virtual machines.

### Install Windows Server 2012 R2 or Windows Server 2016

On one virtual machine, install Windows Server 2012 R2, or Windows Server 2016 Technical Preview 3” to create a computer “CORPDC”.

1. Specify Windows Server 2012 R2 Standard (Server with a GUI) x64 or Windows Server 2016 Technical Preview 3 (Server with Desktop Experience).

2. Review and accept the license terms.

3. Since the disk will be empty, select Custom: Install Windows only and use the uninitialized disk space.

4. Log into that new computer as its administrator. Using **Control Panel**, set its name to CORPDC, and give it a static IP address on the virtual network. Restart the server.

5. After the server has restarted, login as an administrator. Using Control Panel, configure the computer to check for updates, and install any updates needed. Restart the server.

### Add Roles

After the server has restarted, login as an administrator. Using PowerShell, add the Active Directory Domain Services (AD DS), DNS Server and File Server (part of the File and Storage Services section) roles, and promote this server to a domain controller of a new forest “contoso.local”, as described below:

1. While logged in as Administrator, launch PowerShell.

2. Type the following commands.

   `import-module ServerManager`

    `Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools`

    `Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork`

  This will prompt for a Safe Mode Administrator Password to use. Note that warning messages for DNS delegation and cryptography settings will appear. These are normal.

3. After the forest creation is complete, sign out and the server will restart automatically.

4. After the server restarts, login to CORPDC as an administrator of the domain, typically the user *CONTOSO\\Administrator*, which will have the same password as specified when installing Windows on *CORPDC*.

### Create new users and groups

Create new users and groups, including a group named “CorpAdmins”, a user named “Jen”, as well as a group needed for auditing purposes by AD itself. The name of the group must be is the NetBIOS domain name followed by three dollar signs: *“CONTOSO$$$”*. The group scope must be *“Domain local”*, and the group type *“Security”*. This is necessary to enable groups to be created in the PRIV forest in a later step.

1. Launch PowerShell.

2. Type the following commands.

   `import-module activedirectory`

    `New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins`

    `New-ADUser –SamAccountName Jen –name Jen`

    `Add-ADGroupMember –identity CorpAdmins –Members Jen`

    `$jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force`

    `Set-ADAccountPassword –identity Jen –NewPassword $jp`

    `Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"`

    `New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'`

### Configure auditing

1. Go to Start, Administrative Tools (or on Windows Server 2016, Start, Windows Administrative Tools), and launch Group Policy Management.

2. Navigate to Forest: contoso.local, Domains, contoso.local, Domain Controllers, Default Domain Controllers Policy. An informational message will appear.

3. Right-click on Default Domain Controllers Policy and select Edit... in the Right-click menu. A new window will appear.

4. In the Group Policy Management Editor window, under the Default Domain Controllers Policy tree, navigate to and expandComputer Configuration, Policies, Windows Settings, Security Settings, Local Policies, Audit Policy.

5. In the details pane, right click on Audit account management and select Properties in the right-click menu. Click Define these policy settings, put a checkbox on Success, put a checkbox on Failure, click Apply and OK.

6. In the details pane, right click on Audit directory service access and select Properties in the right-click menu. Click Define these policy settings, put a checkbox on Success, put a checkbox on Failure, click Apply and OK.

7. Close the Group Policy Management Editor window, the Group Policy Management window. Then apply the audit settings by launching a PowerShell window and typing:

    `gpupdate /force /target:computer`

  The message “Computer Policy update has completed successfully.” should appear after a few minutes.

### Configure registry settings
Configure registry settings that will be needed for SID History migration, which will be used for privileged access management group creation, and restart the domain controller.

1. Launch PowerShell.

2. Type the following commands that will configure the source domain to permit remote procedure call (RPC) access to the security accounts manager (SAM) database.

   `New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1`

   `Restart-Computer`

This will restart CORPDC. For further information on this registry setting, see this [knowledge base article](http://support.microsoft.com/kb/322970).

### Install Windows 8.1 or Windows 10

On another new virtual machine with no software installed, install Windows 8.1 Enterprise or Windows 10 Enterprise to make a computer *“CORPWKSTN”*.

1. Use Express settings during installation.

2. Note that the installation may not be able to connect to the Internet. Click to Create a local account. Specify a different username; do not use “Administrator” or “Jen”.

3. Using the Control Panel, give this computer a static IP address on the virtual network, and set the interface’s preferred DNS server to be that of the CORPDC server.

4. Using the Control Panel, domain join the CORPWKSTN computer to the contoso.local domain. This will require providing the Contoso domain administrator credentials. Then when this completes, restart the computer CORPWKSTN.

5. After the computer restarts, click the “**Switch user**” icon, click on “**Other user**”. Ensure that the user CONTOSO\\Jen can log into CORPWKSTN.

6. On CORPWKSTN, create and share a new folder named “CorpFS” with the “CorpAdmins” group.

7. On the **Start menu**, type PowerShell and select to Run as Administrator.

8. When the window opens, type the following commands.

   `mkdir c:\corpfs`

   `New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins`

   `$acl = Get-Acl c:\corpfs`

   `$car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")`

   `$acl.SetAccessRule($car)`

   `Set-Acl c:\corpfs $acl`
