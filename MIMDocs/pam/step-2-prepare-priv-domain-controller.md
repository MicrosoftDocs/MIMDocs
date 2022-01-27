---
# required metadata

title: Deploy PAM step 2 - PRIV DC | Microsoft Docs
description: Prepare the PRIV domain controller, which will provide the bastion environment where Privileged Access Management is isolated.
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---
# Step 2 - Prepare the first PRIV domain controller

> [!div class="step-by-step"]
> [« Step 1](step-1-prepare-corp-domain.md)
> [Step 3 »](step-3-prepare-pam-server.md)

In this step, you'll create a new domain that will provide the bastion environment for administrator authentication.  This forest will need at least one domain controller, a member workstation, and at least one member server. The member server will be configured in the next step.

## Create a new Privileged Access Management domain controller

In this section, you'll set up a virtual machine to act as a domain controller for a new forest.

### Install Windows Server 2016 or later

On another new virtual machine with no software installed, install Windows Server 2016 or later to make a computer “PRIVDC”.

1. Select to perform a custom (not upgrade) install of Windows Server. When installing, specify **Windows Server 2016 (Server with Desktop Experience)**; _don't select_ **Data Center or Server Core**.

2. Review and accept the license terms.

3. Since the disk will be empty, select **Custom: Install Windows only** and use the uninitialized disk space.

4. After installing the operating system version, sign in to this new computer as the new administrator. Use Control Panel to set the computer name to *PRIVDC*. In network settings, give it a static IP address on the virtual network, and configure the DNS server to be that of the domain controller installed in the previous step. This will require a server restart.

5. After the server has restarted, sign in as the administrator. Using Control Panel, configure the computer to check for updates, and install any updates needed. This may require a server restart.

### Add roles

Add the Active Directory Domain Services (AD DS) and DNS Server roles.

1. Launch PowerShell as an administrator.

2. Type the following commands to prepare for a Windows Server Active Directory installation.

   ```PowerShell
   import-module ServerManager

   Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools
   ```

### Configure registry settings for SID History migration

Launch PowerShell and type the following command to configure the source domain to permit remote procedure call (RPC) access to the security accounts manager (SAM) database.

```PowerShell
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

## Create a new Privileged Access Management forest

Next, promote the server to a domain controller of a new forest.

In this guide, the name priv.contoso.local is used as the domain name of the new forest.  The name of the forest isn't critical, and it doesn't need to be subordinate to an existing forest name in the organization. However, both the domain and NetBIOS names of the new forest must be unique and distinct from that of any other domain in the organization.

### Create a domain and forest

1. In a PowerShell window, type the following commands to create the new domain.  This will also create a DNS delegation in a superior domain (contoso.local), which was created in a previous step.  If you intend to configure DNS later, then omit the `CreateDNSDelegation -DNSDelegationCredential $ca` parameters.

   ```PowerShell
   $ca= get-credential
   Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
   ```

2. When the popup appears to configure DNS delegation, provide the credentials for the CORP forest administrator, which in this guide was the username CONTOSO\\Administrator, and the corresponding password from step 1.

3. The PowerShell window will prompt you for a Safe Mode Administrator Password to use. Enter a new password twice. Warning messages for DNS delegation and cryptography settings will appear; these are normal.

After the forest creation is complete, the server will restart automatically.

### Create user and service accounts

Create the user and service accounts for MIM Service and Portal setup. These accounts will go in the Users container of the priv.contoso.local domain.

1. After the server restarts, sign in to PRIVDC as the domain administrator (PRIV\\Administrator).

2. Launch PowerShell and type the following commands. The password 'Pass@word1' is just an example and you should use a different password for the accounts.

   ```PowerShell
   import-module activedirectory

   $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

   New-ADUser –SamAccountName MIMMA –name MIMMA

   Set-ADAccountPassword –identity MIMMA –NewPassword $sp

   Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor

   Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp

   Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent

   Set-ADAccountPassword –identity MIMComponent –NewPassword $sp

   Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMSync –name MIMSync

   Set-ADAccountPassword –identity MIMSync –NewPassword $sp

   Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName MIMService –name MIMService

   Set-ADAccountPassword –identity MIMService –NewPassword $sp

   Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName SharePoint –name SharePoint

   Set-ADAccountPassword –identity SharePoint –NewPassword $sp

   Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName SqlServer –name SqlServer

   Set-ADAccountPassword –identity SqlServer –NewPassword $sp

   Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1

   New-ADUser –SamAccountName BackupAdmin –name BackupAdmin

   Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp

   Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

   New-ADUser -SamAccountName MIMAdmin -name MIMAdmin

   Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp

   Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1

   Add-ADGroupMember "Domain Admins" SharePoint

   Add-ADGroupMember "Domain Admins" MIMService
   ```

### Configure auditing and logon rights

You need to set up auditing in order for the PAM configuration to be established across forests.

1. Make sure you're signed in as the domain administrator (PRIV\\Administrator).

2. Go to **Start** > **Windows Administrative Tools** > **Group Policy Management**.

3. Navigate to **Forest: priv.contoso.local** > **Domains** > **priv.contoso.local** > **Domain Controllers** > **Default Domain Controllers Policy**. A warning message will appear.

4. Right-click on **Default Domain Controllers Policy** and select **Edit**.

5. In the Group Policy Management Editor console tree, navigate to **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Local Policies** > **Audit Policy**.

6. In the Details pane, right-click on **Audit account management** and select **Properties**. Click **Define these policy settings**, check the **Success** checkbox, check the **Failure** checkbox, click **Apply** and then **OK**.

7. In the Details pane, right-click on **Audit directory service access** and select **Properties**. Click **Define these policy settings**, check the **Success** checkbox, check the **Failure** checkbox, click **Apply** and then **OK**.

8. Navigate to **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Account Policies** > **Kerberos Policy**.

9. In the Details pane, right-click on **Maximum lifetime for user ticket** and select **Properties**. Click **Define these policy settings**, set the number of hours to *1*, click **Apply** and then **OK**. Note that other settings in the window will also change.

10. In the Group Policy Management window, select **Default Domain Policy**, right-click and select **Edit**.

11. Expand **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Local Policies** and select **User Rights Assignment**.

12. On the Details pane, right-click on **Deny log on as a batch job** and select **Properties**.

13. Check the **Define these Policies Settings** checkbox, click **Add User or Group**, and in the User and group names field, type *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* and click **OK**.

14. Click **OK** to close the window.

15. On the Details pane, right-click on **Deny log on through Remote Desktop Services** and select **Properties**.

16. Click the **Define these Policies Settings** checkbox, click **Add User or Group**, and in the User and group names field, type *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* and click **OK**.

17. Click **OK** to close the window.

18. Close the Group Policy Management Editor window and the Group Policy Management window.

19. Launch a PowerShell window as administrator and type the following command to update the DC from the group policy settings.

    ```cmd
    gpupdate /force /target:computer
    ```

    After a minute, it will complete with the message "Computer Policy update has completed successfully".


### Configure DNS name forwarding on PRIVDC

Using PowerShell on PRIVDC, configure DNS name forwarding in order for the PRIV domain to recognize other existing forests.

1. Launch PowerShell.

2. For each domain at the top of each existing forest, type the following command. In that command, specify the existing DNS domain (such as contoso.local), and the IP addresses of the primary DNS servers of that domain.  

   If you created one domain contoso.local in the previous step with *10.1.1.31* as its IP address, then specify *10.1.1.31* for the CORPDC computer’s virtual network IP address.

   ```PowerShell
   Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
   ```

> [!NOTE]
> The other forests must also be able to route DNS queries for the PRIV forest to this domain controller.  If you have multiple existing Active Directory forests, then you must also add a DNS conditional forwarder to each of those forests.

### Configure Kerberos

1. Using PowerShell, add SPNs so that SharePoint, PAM REST API, and the MIM Service can use Kerberos authentication.

   ```cmd
   setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
   setspn -S http/pamsrv PRIV\SharePoint
   setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
   setspn -S FIMService/pamsrv PRIV\MIMService
   ```

> [!NOTE]
> The next steps of this document describe how to install MIM 2016 server components on a single computer. If you plan to add another server for high availability, then you will need additional Kerberos configuration as described in [FIM 2010: Kerberos Authentication Setup](https://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx).

### Configure delegation to give MIM service accounts access

Perform the following steps on PRIVDC as a domain administrator.

1. Launch **Active Directory Users and Computers**.
2. Right-click on the domain **priv.contoso.local** and select **Delegate Control**.
3. On the Selected Users and Groups tab, click **Add**.
4. On the Select Users, Computers, or Groups window, type *mimcomponent; mimmonitor; mimservice* and click **Check Names**. After the names are underlined, click **OK** then **Next**.
5. In the list of common tasks, select **Create, delete, and manage user accounts** and **Modify the membership of a group**, then click **Next** and **Finish**.

6. Again, right click on domain **priv.contoso.local** and select **Delegate Control**.
7. On the Selected Users and Groups tab, click **Add**.  
8. On the Select Users, Computers, or Groups window, enter *MIMAdmin* and click **Check Names**. After the names are underlined, click **OK** then **Next**.
9. Select **custom task**, apply to **This folder**, with **General permissions**.
10. In the permissions list, select the following permissions:
    - **Read**
    - **Write**
    - **Create all Child Objects**
    - **Delete all Child Objects**
    - **Read All Properties**
    - **Write All Properties**
    - **Migrate SID History**
11. Click **Next**, then **Finish**.

12. Once more, right-click on the domain **priv.contoso.local** and select **Delegate Control**.  
13. On the Selected Users and Groups tab, click **Add**.  
14. On the Select Users, Computers, or Groups window, enter *MIMAdmin* then click **Check Names**. After the names are underlined, click **OK**, then **Next**.  
15. Select **custom task**, apply to **This folder**, then click **only User objects**.    
16. In the permissions list, select **Change password** and **Reset password**. Then click **Next** and then **Finish**.  
17. Close Active Directory Users and Computers.

18.	Open a command prompt.  
19.	Review the access control list on the Admin SD Holder object in the PRIV domains. For example, if your domain was “priv.contoso.local”, type the command:
    ```cmd
    dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local"
    ```
20.	Update the access control list as needed to ensure that the MIM Service and MIM PAM component service can update memberships of groups protected by this ACL.  Type the command:

    ```cmd
    dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimservice:WP;"member"
    dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimcomponent:WP;"member"
    ```

### Configure PAM in Windows Server 2016

Next, authorize the MIM administrators and MIM Service account to create and update shadow principals.

1. Enable the Privileged Access Management features in Windows Server 2016 Active Directory are present and enabled in the PRIV forest.  Launch a PowerShell window as administrator and type the following commands.

   ```
   $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
   Enable-ADOptionalFeature $of -scope ForestOrConfigurationSet -target "priv.contoso.local"
   ```

2. Launch a PowerShell window and type ADSIEdit.

3. Open the Actions menu, click “Connect To”. On the Connection point setting, change the naming context from “Default naming context” to “Configuration” and click OK.

4. After connecting, on the left side of the window below “ADSI Edit”, expand the Configuration node to see “CN=Configuration,DC=priv,....”. Expand CN=Configuration, and then expand CN=Services.

5. Right click on “CN=Shadow Principal Configuration” and click on Properties. When the properties dialog appears, change to the security tab.

6. Click Add. Specify the accounts “MIMService”, as well as any other MIM administrators who will later be performing New-PAMGroup to create additional PAM groups. For each user, in the allowed permissions list, add “Write”, “Create all child objects”, and “Delete all child objects”. Add the permissions.

7. Change to Advanced Security settings. On the line that allows MIMService access, click Edit. Change the “Applies to” setting to “to this object and all descendant objects”. Update this permission setting and close the security dialog box.

8. Close ADSI Edit.

9. Next, authorize the MIM administrators to create and update authentication policy. Launch an elevated **Command prompt** and type the following commands, substituting the name of your MIM administrator account for “mimadmin” in each of the four lines:
    ```
    dsacls "CN=AuthN Policies,CN=AuthN Policy Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

    dsacls "CN=AuthN Policies,CN=AuthN Policy Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g mimadmin:CCDC;msDS-AuthNPolicy

    dsacls "CN=AuthN Silos,CN=AuthN Policy Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

    dsacls "CN=AuthN Silos,CN=AuthN Policy Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g mimadmin:CCDC;msDS-AuthNPolicySilo
    ```

10. Restart the PRIVDC server so that these changes take effect.

## Prepare a PRIV workstation

Follow these instructions to prepare a workstation.  This will be joined to the PRIV domain for performing maintenance of PRIV resources (such as MIM).

### Install Windows 10 Enterprise

On another new virtual machine with no software installed, install Windows 10 Enterprise to make a computer *“PRIVWKSTN”*.

1. Use Express settings during installation.

2. Note that the installation may not be able to connect to the Internet. Click to **Create a local account**. Specify a different username; do not use “Administrator” or “Jen”.

3. Using the Control Panel, give this computer a static IP address on the virtual network, and set the interface’s preferred DNS server to be that of the PRIVDC server.

4. Using the Control Panel, domain join the PRIVWKSTN computer to the priv.contoso.local domain. This will require providing the PRIV domain administrator credentials. When this completes, restart the computer PRIVWKSTN.

5. Install the [Visual C++ 2013 Redistributable Packages](https://www.microsoft.com/download/details.aspx?id=40784) for 64-bit Windows.

If you want more details, see [securing privileged access workstations](https://technet.microsoft.com/library/mt634654.aspx).

In the next step, you'll prepare a PAM server.

> [!div class="step-by-step"]
> [« Step 1](step-1-prepare-corp-domain.md)
> [Step 3 »](step-3-prepare-pam-server.md)
