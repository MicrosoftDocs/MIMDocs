---
title: Step 2 - Prepare the PRIV domain controller
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
author: Kgremban
---
# Step 2 - Prepare the PRIV domain controller
## Create a new privileged access management forest

In this step you will create a new domain for a new privileged access management forest.

### Install Windows Server 2012 R2
On another new virtual machine with no software installed, install Windows Server 2012 R2 to make a computer “PRIVDC”.

1. Select to perform a custom (not upgrade) install of Windows Server. When installing, specify *Windows Server 2012 R2 Standard (Server with a GUI) x64*; **do not** select *Data Center or Server Core*.

2. Review and accept the license terms.

3. Since the disk will be empty, select *Custom: Install Windows only* and use the uninitialized disk space.

4. After installing the operating system version, log in to this new computer as the new administrator. Use Control Panel to set the computer name to “PRIVDC”, give it a static IP address on the virtual network, and configure the DNS server to be that of the domain controller installed in the previous step. This will require a server restart.

5. After the server has restarted, login as the administrator. Using Control Panel, configure the computer to check for updates, and install any updates needed. This may require a server restart.

### Add roles
Add the Active Directory Domain Services (AD DS) and DNS Server roles, and promote the server to a domain controller of a new forest.

**NOTE:** In this document, the name *priv.contoso.local* is used as the domain name of the new forest.  The name of the forest is not critical, and it does not need to be subordinate to an existing forest name in the organization. However, both the domain and NetBIOS names of the new forest must be unique and distinct from that of any other domain in the organization.

1. While logged in as Administrator, launch PowerShell.
2. Type the following commands.

   `import-module ServerManager`

    `Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools`

   `$ca= get-credential`

   `Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca`

3. When the popup appears, provide the credentials for the CORP forest administrator (e.g., the username CONTOSO\\Administrator and the corresponding password from step 1). Then this will prompt within the PowerShell window for a Safe Mode Administrator Password to use: enter a new password twice. Note that warning messages for DNS delegation and cryptography settings will appear; these are normal.

After the forest creation is complete, the server will restart automatically.

### Create user and service accounts
Create the user and service accounts, which will be needed during MIM Service and Portal setup, in Users container of the **priv.contoso.local** domain.

1. After restarting, log on to PRIVDC as the domain administrator (PRIV\\Administrator).

2. Type the following commands.

    `import-module activedirectory`

    `$sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force`

    `New-ADUser –SamAccountName MIMMA –name MIMMA`

    `Set-ADAccountPassword –identity MIMMA –NewPassword $sp`

    `Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor`

    `Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp`

    `Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1`

    `New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent`

    `Set-ADAccountPassword –identity MIMComponent –NewPassword $sp`

   `Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1`

    `New-ADUser –SamAccountName MIMSync –name MIMSync`

    `Set-ADAccountPassword –identity MIMSync –NewPassword $sp`

    `Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName MIMService –name MIMService`

   `Set-ADAccountPassword –identity MIMService –NewPassword $sp`

    `Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName SharePoint –name SharePoint`

   `Set-ADAccountPassword –identity SharePoint –NewPassword $sp`

   `Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName SqlServer –name SqlServer`

   `Set-ADAccountPassword –identity SqlServer –NewPassword $sp`

   `Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName BackupAdmin –name BackupAdmin`

   `Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp`

   `Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1`

   `New-ADUser -SamAccountName MIMAdmin -name MIMAdmin`

   `Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp`

   `Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1`

   `Add-ADGroupMember "Domain Admins" SharePoint`

    `Add-ADGroupMember "Domain Admins" MIMService`

### Configure auditing and logon rights

1. Ensure you are logged on as the domain administrator (such as PRIV\\Administrator).

2. Go to **Start** » **Administrative Tools** » **Group Policy Management**.

3. Navigate to Forest: *priv.contoso.local* » **Domains** » *priv.contoso.local* » **Domain Controllers** » **Default Domain Controllers Policy**. A warning message will appear.

4. Right-click on **Default Domain Controllers Policy** and select **Edit** in the menu.

5. In the **Group Policy Management Editor** console tree, navigate to **Computer Configuration** » **Policies** » **Windows Settings** » **Security Settings** » **Local Policies** » **Audit Policy**.

6. In the **Details** pane, right-click on **Audit account management** and select **Properties** in the menu. Click **Define these policy settings**, check the **Success** checkbox, check the **Failure** checkbox, click **Apply** and then **OK**.

7. In the **Details** pane, right-click on **Audit directory service access** and select **Properties** in the menu. Click **Define these policy settings**, check the **Success** checkbox, check the **Failure** checkbox, click **Apply** and then **OK**.

8. Navigate to **Computer Configuration** » **Policies** » **Windows Settings** » **Security Settings** » **Account Policies** » **Kerberos Policy**.

9. In the **Details** pane, right-click on **Maximum lifetime for user ticket** and select **Properties** in the menu. Click **Define these policy settings**, set the number of hours to *1*, click **Apply** and then **OK**. Note that other settings in the window will also change.

10. In the **Group Policy Management** window, select **Default Domain Policy**, right-click and select **Edit**. The **Group Policy Management Editor** window will appear.

11. Expand **Computer Configuration** » **Policies** » **Windows Settings** » **Security Settings** » **Local Policies** and select **User Rights Assignment**.

12. On the **Details** pane, right-click on **Deny log on as a batch job**, and select **Properties**.

13. Check the **Define these Policies Settings** checkbox, click **Add User or Group**, and in the **User and group** names field, type *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* and click **OK**.

14. Click **OK** to close the **Deny log on as a batch job Properties** window.

15. On the **Details** pane, right-click on **Deny log on through Remote Desktop Services**, and select **Properties**.

16. Click the **Define these Policies Settings** checkbox, click **Add User or Group**, and in the **User and group names** field, type *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* and click **OK**.

17. Click **OK** to close the **Deny log on through Remote Desktop Services Properties** window.

18. Close the **Group Policy Management Editor** window and the **Group Policy Management** window.

19. Launch a PowerShell window as administrator and type the following command to update the DC from the group policy settings.

    `gpupdate /force /target:computer`

  After a minute, it will complete with the message “Computer Policy update has completed successfully.”

### Configure registry settings needed for SID History migration.

Launch PowerShell and type the following commands to configure the source domain to permit remote procedure call (RPC) access to the security accounts manager (SAM) database.


```
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

### Configure DNS name forwarding on PRIVDC

Using PowerShell on PRIVDC, configure DNS name forwarding. Specify *contoso.local* for the DNS domain, and the CORPDC computer’s virtual network IP address as an IP address of the master server.

1. Launch PowerShell and type the following command, changing the IP address from "10.1.1.31" to that of the CORPDC computer’s virtual network IP address.

   `Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31`

2. Using PowerShell, add SPNs to enable Kerberos authentication to be used by SharePoint and PAM REST API and the MIM Service (SharePoint will be configured in Step 3 below).

    `setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint`

    `setspn -S http/pamsrv PRIV\SharePoint`

   `setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService`

   `setspn -S FIMService/pamsrv PRIV\MIMService`

**NOTE:** This guide describes how to get started installing MIM 2016 server components on a single system for testing purposes.  Additional Kerberos configuration is required to install MIM 2016 server components on multiple systems for high availability, such as described [here](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx).

### Configure delegation.

1. Launch **Active Directory Users and Computers**.

2. Right-click on the domain *priv.contoso.local* and select **Delegate Control**.

3. On the **Selected users and groups** tab, click **Add**.

4. On the **Select Users, Computers, or Groups** window, type *mimcomponent; mimmonitor; mimservice* and click **Check Names**. After the names are underlined, click **OK**, then click **Next**.

5. In the list of common tasks, select **Create, delete, and manage user accounts** and **Modify the membership of a group**, then click **Next** and then **Finish**.

6. Right click on *domain priv.contoso.local* and select **Delegate Control**.

7. On the **Selected users and groups** tab, click **Add**.

8. On the **Select Users, Computers, or Groups** popup, enter *MIMAdmin* and click **Check Names**.

9. After the names are underlined, click **OK**, then click **Next**.

10. Select **custom task**, apply to **This folder**, with **General permissions**.  

11. In the permissions list, select the following: **Read**, **Write**, **Create all Child Objects**, **Delete all Child Objects**, **Read All Properties**, **Write All Properties**, and **Migrate SID History**. Click **Next** then **Finish**.

12. Again, right-click on the *domain priv.contoso.local* and select **Delegate Control**.

13. On the **Selected users and groups** tab, click **Add**.

14. On the **Select Users, Computers, or Groups** popup, enter *MIMAdmin* then click **Check Names**.

15. After the names are underlined, click **OK**, then **Next**.

15. Select **custom task**, apply to **This folder**, then click **only User objects**.  

16. In the permissions list, select **Change password** and **Reset password**. Then click **Next** and then **Finish**.

17. Close **Active Directory Users and Computers**.

16. Restart the *PRIVDC* server so that these changes take effect.
