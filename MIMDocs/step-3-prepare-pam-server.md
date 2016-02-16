---
title: Step 3 – Prepare a PAM server
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
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
robots: noindex,nofollow
---
# Step 3 – Prepare a PAM server

1.  On a third virtual machine, install Windows Server 2012 R2, specifically Windows Server 2012 R2 Standard (Server with a GUI) x64, to make a new computer “*PAMSRV*”.   Note that since SQL Server and SharePoint 2013 will be installed on this computer, it requires a minimum of 8GB of RAM.

    1.  Specify Windows Server 2012 R2 Standard (Server with a GUI) x64.

        ![](../Image/PAM_GS_Select_WS2012.png)

    2.  Review and accept the license terms.

    3.  Since the disk will be empty, select **Custom: Install Windows only** and use the uninitialized disk space.

2.  Log into that new computer as its administrator.  Using Control Panel, give it a static IP address on the virtual network, configure that network interface to send DNS queries to the IP address of **PRIVDC** and set the computer name to **PAMSRV**.  This will require a server restart.

3.  If the virtual network does not provide Internet connectivity, add an additional network interface to the computer that provides a connection to the Internet.  This will be needed for SharePoint installation, and can be disabled after this step is completed.

4.  After the server has restarted, login as the administrator. Using Control Panel, configure the computer to check for updates, and install any updates needed.  This may require a server restart.

5.  After the server restarts, login as Administrator, open the Control Panel and join **PAMSRV** to the **PRIV** domain (*priv.contoso.local*).  This will require providing the username and credentials of a **PRIV** domain administrator such as *PRIV\Administrator*. After the welcome message appears, close the dialog box and restart this server.

6.  Start the computer **PAMSRV** and login to it as a **PRIV** domain administrator such as *PRIV\Administrator*.

7.  Add the Web Server (IIS) and Application Server roles, the .NET Framework 3.5 Features, the Active Directory module for Windows PowerShell, and other features required by SharePoint

    1.  While logged in as Administrator, launch PowerShell.

    2.  Type the following commands.   Note that it may be necessary to specify a different location for the source files for .NET Framework 3.5 features. These features are typically not present when Windows Server installs, but are available in the side-by-side (SxS) folder on the OS install disk sources folder, e.g., “*d:\Sources\SxS\*”.

        ```
        import-module ServerManager
        Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
        ```

8.  Configure the server security policy to allow the newly-created accounts to run as services.

    1.  Launch the **Local Security Policy** program.

    2.  Navigate to **Local Policies »  User Rights Assignment**.

    3.  On the **details pane**, right click on **Log on as a service**, and select **Properties**.

    4.  Click **Add User or Group**, and in the User and group names, type *priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer*, click **Check Names**, and click **OK**.

    5.  Click **OK** to close the Log on as a service Properties window.

    6.  On the **details pane**, right click on **Deny access to this computer from the network**, and select **Properties**.

    7.  Click **Add User or Group**, and in the User and group names, type *priv\mimmonitor; priv\MIMService; priv\mimcomponent* and click **OK**.

    8.  Click **OK** to close the Deny access to this computer from the network Properties window.

    9. On the **details pane**, right click on **Deny log on locally**, and select **Properties**.

    10. Click **Add User or Group**, and in the User and group names, type *priv\mimmonitor; priv\MIMService; priv\mimcomponent* and click **OK**.

    11. Click **OK** to close the Deny log on locally Properties window.

    12. **Close** the Local Security Policy window.

9. Change the IIS configuration to allow applications to use Windows Authentication mode.

    1.  Open a PowerShell window

    2.  Stop IIS and unlock the application host settings using these commands

        ```
        iisreset /STOP
        C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
        iisreset /START
        ```

    3.  Alternatively, using a text editor such as Notepad, open this file:
        C:\Windows\System32\inetsrv\config\applicationHost.config 
        and on line 82 of that file, replace the tag value of *overrideModeDefault*:

        &lt;section name="windowsAuthentication" overrideModeDefault="Deny" /&gt;

        with

        &lt;section name="windowsAuthentication" overrideModeDefault="Allow" /&gt;

        Then save the file, and restart IIS with the command *iisreset /START*

10. Install **SQL Server 2012 Service Pack 1** or later, or **SQL Server 2014**. The following steps assume SQL 2014.

    1.  Launch PowerShell as a domain administrator.

    2.  Change to the directory where the SQL Server setup program is located.

    3.  Type the following commands.

        ```
        .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\Administrator"
        ```

11. Using the **SharePoint Foundation 2013 with SP1 installer**, install SharePoint’s software prerequisites on *PAMSRV*.  Note that this will cause the server to restart, and will also require Internet connectivity for this computer for the installer to download its prerequisites.

    1.  Launch PowerShell as a domain administrator.

    2.  Change to the directory where SharePoint was unpacked.

    3.  Type the following command.

        ```
        .\prerequisiteinstaller.exe
        ```

12. After the SharePoint prerequisites are installed, install **SharePoint Foundation 2013 with SP1**.

    1.  Launch PowerShell as a domain administrator.

    2.  Change to the directory where SharePoint was unpacked.

    3.  Type the following command.

        ```
        .\setup.exe
        ```

    4.  Select the complete server type.

    5.  After the install completes, select to run the wizard.

13. Run the **SharePoint Products Configuration Wizard** to configure SharePoint.

    1.  On the "Connect to a server farm" tab, change to create a new server farm.

    2.  Specify **PAMSRV** as the database server for the configuration database, and **PRIV\SharePoint** as the database access account for SharePoint to use.

    3.  Specify a password as the farm security passphrase (it will not be used later in this lab environment).

    4.  For this test lab environment, accept the rest of the SharePoint configuration wizard default settings.

    5.  When the configuration wizard completes configuration task 10 of 10, click **Finish** and a web browser will open.

    6.  In the Internet Explorer popup, authenticate as **PRIV\Administrator** (or the equivalent domain administrator account) to proceed.

    7.  Start the wizard (within the web app) to configure the SharePoint farm.

    8.  Select to use the existing managed account (**PRIV\SharePoint**), and click **Next**.

    9. Once the creating a site collection window appears, click **Skip**.  Then click **Finish**.

14. After the wizards complete, use PowerShell to create a **SharePoint Foundation 2013 Web Application** to host the MIM Portal.  Note that as this is a test lab environment, SSL will not be enabled.

    1.  Launch  SharePoint 2013 Management Shell and run the following PowerShell script:

        ```
        $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
        New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"            -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
        ```
        Note that a warning message will appear that Windows Classic authentication method is being used, and it may take several minutes for the final command to return.  When completed, the output will indicate the URL of the new portal.  Keep the SharePoint 2013 Management Shell window open as it will be needed in a subsequent task.

15. Next, create a **SharePoint Site Collection** associated with that web application to host the MIM Portal.

    1.  Launch  **SharePoint 2013 Management Shell**, if not already open, and run the following PowerShell script

        ```
        $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
        $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82 
        New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\Administrator                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin 
        $s = SpSite($w.Url)
        $s.AllowSelfServiceUpgrade = $false
        $s.CompatibilityLevel
        ```
        Verify that the result of the *CompatibilityLevel* variable is “14”.  ([See http://technet.microsoft.com/library/jj863242(v=ws.10).aspx](http://technet.microsoft.com/library/jj863242%28v=ws.10%29.aspx) for more information). If the result is “15”, then the site collection was not created for the 2010 experience version; delete the site collection and recreate it.

    2.  Disable SharePoint server-side viewstate, and the SharePoint task "*Health Analysis Job (Hourly, Microsoft SharePoint Foundation Timer, All Servers*", by running the following PowerShell commands in the **SharePoint 2013 Management Shell**:

        ```
        $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
        $contentService.ViewStateOnServer = $false;
        $contentService.Update();
        Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
        ```

16. Open a new web browser tab, navigate to *http://pamsrv.priv.contoso.local:82/* and login as *PRIV\Administrator*.  An empty SharePoint site named “MIM Portal” will be shown. Then in Internet Explorer, open **Internet Options**, change to the **Security tab,** select **Local intranet**, and add the web site. (Note that if sign in fails, the Kerberos SPNs created earlier in step 2 might need to be updated.)

17. Using **Services** (located in Administrative Tools), start the **SharePoint Administration** service, if not already running.

