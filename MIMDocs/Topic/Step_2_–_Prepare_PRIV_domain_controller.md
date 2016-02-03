---
title: Step 2 – Prepare PRIV domain controller
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
ms.assetid: 10eb5639-56e0-4208-8f71-58b549ca706e
robots: noindex,nofollow
---
# Step 2 – Prepare PRIV domain controller
In this step you will create a new domain for a new privileged access management forest.

1.  On another new virtual machine with no software installed, install Windows Server 2012 R2 to make a computer “*PRIVDC*”.

    1.  Select to perform a custom (not upgrade) install of Windows Server.

    2.  When installing, specify **Windows Server 2012 R2 Standard (Server with a GUI) x64**; do not select Data Center or Server Core.

        ![](../Image/PAM_GS_Select_WS2012.png)

    3.  Review and accept the license terms.

    4.  Since the disk will be empty, select **Custom: Install Windows only** and use the uninitialized disk space.

2.  After installing the operating system version, log in to this new computer as the new administrator.  Use Control Panel to set the computer name to “*PRIVDC*”, give it a static IP address on the virtual network, and configure the DNS server to be that of the domain controller installed in the previous step.  This will require a server restart.

3.  After the server has restarted, login as the administrator. Using Control Panel, configure the computer to check for updates, and install any updates needed.  This may require a server restart.

4.  Add the **Active Directory Domain Services** (AD DS) and **DNS Server** roles, and promote the server to a domain controller of a new forest.

    1.  While logged in as Administrator, launch PowerShell.

    2.  Type the following commands.

        ```
        import-module ServerManager
        Install-WindowsFeature AD-Domain-Services,DNS –restart                            –IncludeAllSubFeature -IncludeManagementTools
        $ca= get-credential
        Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
        ```
        When the popup appears, provide the credentials for the CORP forest administrator (e.g., the username CONTOSO\Administrator and the corresponding password from step 1).  Then this will prompt within the PowerShell window for a Safe Mode Administrator Password to use: enter a new password twice.  Note that warning messages for DNS delegation and cryptography settings will appear; these are normal.

    3.  After the forest creation is complete, the server will restart automatically.

5.  Create the user and service accounts, which will be needed during MIM Service and Portal setup, in **Users** container of the *priv.contoso.local domain*.

    1.  After restarting, log on to *PRIVDC* as the domain administrator (*PRIV\Administrator*).

    2.  Type the following command to update the DC from the group policy settings.

        ```
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
        Add-ADGroupMember "Domain Admins" SharePoint
        Add-ADGroupMember "Domain Admins" MIMService
        ```

6.  Configure auditing and logon rights.

    1.  Ensure you are logged on as the domain administrator (such as *PRIV\Administrator*).

    2.  Go to **Start » Administrative Tools »  Group Policy Management**.

    3.  Navigate to **Forest**: *priv.contoso.local, Domains, priv.contoso.local, Domain Controllers, Default Domain Controllers Policy*. Note that a warning message will appear.

    4.  Right-click on **Default Domain Controllers Policy** and select **Edit** in the right-click menu.

    5.  In the **Group Policy Management Editor** console tree, navigate to *Computer Configuration, Policies, Windows Settings, Security Settings, Local Policies, Audit Policy*.

    6.  In the **details pane**, right click on **Audit account management** and select **Properties** in the right-click menu. Click **Define these policy settings**, put a checkbox on Success, put a checkbox on **Failure**, click **Apply** and **OK**.

    7.  In the **details pane**, right click on **Audit directory service access** and select **Properties** in the right-click menu. Click **Define these policy settings**, put a checkbox on **Success**, put a checkbox on **Failure**, click **Apply** and **OK**.

    8.  Close the **Group Policy Management Editor window**.

    9. In the **Group Policy Management** window, select **Default Domain Policy**, right click and select **Edit**. The **Group Policy Management Editor** window will appear.

    10. Expand *Computer Configuration, Policies, Windows Settings, Security Settings, Local Policies* and select **User Rights Assignment**.

    11. On the **details pane**, right click on **Deny log on as a batch job**, and select **Properties**.

    12. Click the **Define these Policies Settings** checkbox, click **Add User or Group**, and in the **User and group names**, type p*riv\mimmonitor; priv\MIMService; priv\mimcomponent* and click **OK**.

    13. Click **OK** to close the Deny log on as a batch job Properties window.

    14. On the **details pane**, right click on **Deny log on through Remote Desktop Services**, and select **Properties**.

    15. Click the **Define these Policies Settings** checkbox, click **Add User or Group**, and in the **User and group name**s, *type priv\mimmonitor; priv\MIMService; priv\mimcomponent* and click **OK**.

    16. Click **OK** to close the Deny log on through Remote Desktop Services Properties window.

    17. Close the **Group Policy Management Editor** window and the **Group Policy Management** window.

7.  Launch a PowerShell window as administrator and type the following command to update the DC from the group policy settings.

    ```
    gpupdate /force /target:computer
    ```
    After a minute, it will complete with the message “Computer Policy update has completed successfully.”

8.  Configure registry settings needed for SID History migration. Launch PowerShell and type the following commands that will configure the source domain to permit remote procedure call (RPC) access to the security accounts manager (SAM) database.

    ```
    New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
    ```

9. Using PowerShell on *PRIVDC*, configure DNS name forwarding.  Specify *contoso.local* for the DNS domain, and the *CORPDC* computer’s virtual network IP address as an IP address of the master server. Launch PowerShell and type the following command, changing the IP address from "10.1.1.31" to that of the *CORPDC* computer’s virtual network IP address.

    ```
    Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
    ```

10. Using PowerShell, add SPNs to enable Kerberos authentication to be used by SharePoint and PAM REST API and the MIM Service (SharePoint will be configured in Step 3 below).

    ```
    setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
    setspn -S http/pamsrv PRIV\SharePoint
    setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
    ```

11. Configure delegation.

    1.  Launch **Active Directory Users and Computers**.

    2.  Right click on the **domain priv.contoso.local** and select **Delegate Control**.

    3.  On the **Selected users and groups** tab, click **Add**.

    4.  On the **Select Users, Computers, or Groups** popup, type *mimcomponent; mimmonitor* and click **Check Names**.  After the names are underlined, click **OK**, then click **Next**.

    5.  In the list of common tasks, select "**Create, delete, and manage user accounts**" and "**Modify the membership of a group**", then click **Next** and click **Finish**.

    6.  Close **Active Directory Users and Computers**.

12. Restart the PRIVDC server so that these changes take effect.

