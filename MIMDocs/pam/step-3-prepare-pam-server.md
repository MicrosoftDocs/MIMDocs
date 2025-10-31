---
# required metadata

title: Deploy PAM step 3 – PAM server | Microsoft Docs
description: Prepare a PAM server that will host both SQL and SharePoint for your Privileged Access Management deployment.
keywords:
author: henrymbuguakiarie
ms.author: henrymbuguakiarie
ms.date: 04/08/2025
ms.topic: article
ms.service: microsoft-identity-manager

ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7

# optional metadata

ROBOTS: noindex,nofollow
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---
# Step 3 – Prepare a PAM server

> [!div class="step-by-step"]
> [« Step 2](step-2-prepare-priv-domain-controller.md)
> [Step 4 »](step-4-install-mim-components-on-pam-server.md)

## Install Windows Server 2016 or 2019

On a third virtual machine, install Windows Server 2016 or 2019, to make *PAMSRV*. Since SQL Server MIM Service, and optionally SharePoint 2013, will be installed on this computer, it requires at least 8 GB of RAM.

1. When installing, specify **Windows Server 2016 (Server with Desktop Experience)**.

2. Review and accept the license terms.

3.  Select **Custom: Install Windows only** and use the **uninitialized disk space**, since the disk will be empty.

4.  Log in to that new computer as its administrator.  Using Control Panel, give it a static IP address on the virtual network, configure that network interface to send DNS queries to the IP address of PRIVDC and set the computer name to *PAMSRV*.  This will require a server restart.

5.  If the virtual network doesn't provide Internet connectivity, add an additional network interface to the computer that provides a connection to the Internet.  This will be only needed to download updates and for the SharePoint installation, and can be disabled after this step is completed.

6.  Wait for the server to restart. After the server has restarted, sign in as the administrator. Using Control Panel, configure the computer to check for updates, and install any updates needed.  This may require a server restart.

7.  Wait for the server to restart. After the server restarts, sign in as Administrator, open the Control Panel and join PAMSRV to the PRIV domain (priv.contoso.local).  This will require providing the username and credentials of a PRIV domain administrator (PRIV\Administrator). After the welcome message appears, close the dialog box and restart this server.


### Add the web server (IIS) and application server roles

Add the Web Server (IIS) role, the .NET Framework 3.5 and 4.6 Features, and the Active Directory module for Windows PowerShell. If you plan to install SharePoint, also add other features required by SharePoint.

1.  Sign in as a PRIV domain administrator (PRIV\Administrator) and launch PowerShell.

2.  Type the following commands. Note that it may be necessary to specify a different location for the source files for .NET Framework 3.5 features. These features are typically not present when Windows Server installs, but are available in the side-by-side (SxS) folder on the OS install disk sources folder, for example, `d:\Sources\SxS\`.

    ```PowerShell
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,
    rsat-ad-powershell,Web-Mgmt-Tools,
    Windows-Identity-Foundation,Server-Media-Foundation,
    Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

### Configure the server security policy

Configure the server security policy to allow the newly created accounts to run as services.

1.  Launch the **Local Security Policy** program.
2.  Navigate to **Local Policies** > **User Rights Assignment**.
3.  View the details pane, right-click on **Log on as a service**, and select **Properties**.
4.  Click **Add User or Group**, and in the User and group names, type *priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer*. Click **Check Names**, and click **OK**.

5.  Select **OK** to close the properties window.
6.  View the details pane, right-click on **Deny access to this computer from the network**, and select **Properties**.
7.  Click **Add User or Group**, and in the User and group names, type *priv\mimmonitor; priv\MIMService; priv\mimcomponent* and click **OK**.
8.  Select **OK** to close the properties window.

9. View the details pane, right-click on **Deny log on locally**, and select **Properties**.
10. Click **Add User or Group**, and in the User and group names, type *priv\mimmonitor; priv\MIMService; priv\mimcomponent* and click **OK**.
11. Select **OK** to close the properties window.
12. Close the Local Security Policy window.

13. Launch Control Panel and switch to **User Accounts**.
14. Click **Give others access to this computer**.
15. Click **Add**, enter the user *MIMADMIN* in the domain *PRIV*, and on the next screen in the wizard, click **Add this user as an Administrator**.
16. Click **Add**, enter the user *SharePoint* in the domain *PRIV*, and on the next screen in the wizard, click **Add this user as an Administrator**.
17. Close Control Panel.

### Change the IIS configuration

There are two ways to change the IIS configuration to allow applications to use Windows Authentication mode. Make sure you're signed in as MIMAdmin and then follow one of these options.

If you want to use PowerShell:

1. Right-click on PowerShell and select **Run as administrator**.
2. Stop IIS and unlock the application host settings using these commands.

   ```CMD
   iisreset /STOP
   C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
   iisreset /START
   ```

If you want to use a text editor such as Notepad:

1. Open the file **C:\Windows\System32\inetsrv\config\applicationHost.config**.
2. Scroll down to line 82 of that file. The tag value of **overrideModeDefault** should be `<section name="windowsAuthentication" overrideModeDefault="Deny" />`.
3. Change the value of **overrideModeDefault** to *Allow*.
4. Save the file, and restart IIS with the PowerShell command `iisreset /START`.

## Install prerequisite libraries

1. Install the [Visual C++ 2013 Redistributable Packages](https://www.microsoft.com/download/details.aspx?id=40784) for Windows Server 64-bit.

2. If using TLS 1.2 or FIPS mode in the PRIV domain, then see [MIM 2016 SP2 in "TLS 1.2 only" or FIPS-mode environments](../preparing-tls.md) for more prerequisites.

## Install SQL Server

If SQL Server is not in the bastion environment already, install either SQL Server 2012 (Service Pack 1 or later), SQL Server 2014 or later. The following steps assume SQL 2014.

1. Make sure you are signed in as MIMAdmin.
2. Right-click on PowerShell and select **Run as administrator**.
3. Navigate to the directory where the SQL Server setup program is located.
4. Type the following command.
    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1" /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\MIMAdmin"
    ```

## Change update settings

1. Open Settings, and navigate to **Windows Update**.
2. Check for new updates and make sure any pending important updates for Windows Server or SQL Server are installed before continuing.

## Install SharePoint Server 2016 or 2019 (only required for MIM Portal)

The following sections in this article are prerequisites only needed if installing the MIM Portal.  If you are not going to be installing MIM Portal, then continue with  [Step 4](step-4-install-mim-components-on-pam-server.md) to install the MIM other components.

Use the guide for [Preparing Sharepoint](/microsoft-identity-manager/prepare-server-sharepoint) to install SharePoint Server 2016 or 2019.

## Set the website as the local intranet

1. Launch Internet Explorer and open a new web browser tab.
2. Navigate to `http://pamsrv.priv.contoso.local:82/` and sign in as PRIV\MIMAdmin.  An empty SharePoint site named "MIM Portal" will be shown.
3. In Internet Explorer open **Internet Options**, change to the **Security** tab, select **Local intranet**, and add the URL `http://pamsrv.priv.contoso.local:82/`.

If sign in fails, the Kerberos SPNs created earlier in [Step 2](step-2-prepare-priv-domain-controller.md) might need to be updated.

## Start the SharePoint administration service

Using **Services** (located in Administrative Tools), start the **SharePoint Administration** service, if not already running.

In Step 4, you'll start installing the MIM components onto the PAM server.

> [!div class="step-by-step"]
> [« Step 2](step-2-prepare-priv-domain-controller.md)
> [Step 4 »](step-4-install-mim-components-on-pam-server.md)
