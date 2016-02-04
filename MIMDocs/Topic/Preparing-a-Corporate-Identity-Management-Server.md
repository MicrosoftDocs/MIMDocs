---
title: Preparing a Corporate Identity Management Server
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
ms.assetid: a79eec00-022f-48d5-88e0-2743fe8af85d
author: Kgremban
---
# Preparing a Corporate Identity Management Server

## Deploy and Configure Windows Server 2012 R2

1.  Create a Windows Server 2012 R2 machine, with a minimum of 8GB of RAM. When installing specify "Windows Server 2012 R2 Standard (Server with a GUI) x64" edition.

2.  Log into the new computer as its administrator.

3.  Using the Control Panel, give it a static IP address on the network, configure that network interface to send DNS queries to the IP address of domain controller in the previous step, and set the computer name to CORPIDM.  This will require a server restart.

4.  If the computer is on a virtual network which does not provide Internet connectivity, add an additional network interface to the computer that provides a connection to the Internet.  This will be needed for SharePoint installation, and can be disabled after this step is completed.

5.  Open the Control Panel and join it to the same domain as was just configured (*contoso.local*).  This will require providing the username and credentials of a domain administrator such as *Contoso\Administrator*.  After the welcome message appears, close the dialog box and restart this server again.

6.  Log into  the computer *CorpIDM* as a domain administrator such as *Contoso\Administrator*.

7.  Launch a PowerShell window as administrator and type the following command to update the computer from the group policy settings.
    `gpupdate /force /target:computer`

    After no more than a minute, it will complete with the message “Computer Policy update has completed successfully.

8.  Add the **Web Server (IIS)** and **Application Server** roles, the **.NET Framework** 3.5, 4.0, and 4.5 features, the **Active Directory** module for Windows PowerShell.

    ![](../Image/MIM_DeployWS2.png)

9. Run **Windows PowerShell** as an administrator.

    Type the following commands.   Note that it may be necessary to specify a different location for the source files for **.NET Framework** 3.5 features. These features are typically not present when Windows Server installs, but are available in the side-by-side (SxS) folder on the OS install disk sources folder, e.g., “*d:\Sources\SxS\*”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

10. Configure the server security policy to allow the newly-created accounts to run as services.

    -   Launch the Local Security Policy program

    -   Navigate to **Local Policies, User Rights Assignment**.

    -   On the details pane, right click on **Log on as a service**, and select **Properties**.

    -   Click **Add User or Group**, and in **User and group names**, type`contoso\mimsync; contoso\mimma; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\mimsspr`, click **Check Names**, and click **OK**.

    -   Click **OK** to close the Log on as a service Properties window.

    -   On the details pane, right click on **Deny access to this computer from the network**, and select Properties.

    -   Click **Add User or Group**, and in the User and group names, type contoso\MIMSync; contoso\MIMService and click OK.

    -   Click **OK** to close the Deny access to this computer from the network Properties window

    -   On the details pane, right click on **Deny log on locally**, and select Properties.

    -   Click **Add User or Group**, and in the User and group names, type c`ontoso\MIMSync; contoso\MIMService` and click OK

    -   Click **OK** to close the Deny log on locally Properties window

    -   Close the Local Security Policy window

11. Change the **IIS Windows Authentication mode.**

    1.  Open a PowerShell window.

    2.  Stop IIS using the command *iisreset /STOP*

        ```
        iisreset /STOP
        C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
        iisreset /START
        ```

## Deploy SQL Server
Install **SQL Server 2014 Standard Edition**.

-   Launch **PowerShell** as a domain administrator.

-   Change to the directory where the SQL Server setup program is located.

-   Type the following commands.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

## Deploy SharePoint

1.  Install **SharePoint Foundation 2013 with SP1**.

    > [!NOTE]
    > It will require Internet connectivity for the installer to download its prerequisites.

    The server will restart at the end of the installation.

2.  Launch **PowerShell** as a domain administrator.

    -   Change to the directory where SharePoint was unpacked.

    -   Type the following command.

        ```
        .\prerequisiteinstaller.exe
        ```

3.  After **SharePoint** prerequisites are installed, install **SharePoint Foundation 2013 with SP1** by typing the following command:

    ```
    .\setup.exe
    ```

4.  Select the complete server type.

5.  After the install completes, run the wizard.

6.  Run the **SharePoint Products Configuration Wizard** to configure SharePoint.

    -   On the **Connect to a server farm** tab, change to create a new server farm.

    -   Specify this server as the database server for the configuration database, and *contoso\SharePoint* as the database access account for SharePoint to use.

    -   Specify a password as the farm security passphrase (it will not be used later in this lab environment).

    -   When the configuration wizard completes configuration task 10 of 10, click Finish and a web browser will open.

    -   In the Internet Explorer popup, authenticate as *contoso\Administrator* (or the equivalent domain administrator account) to proceed.

    -   Start the wizard (within the web app) to configure the SharePoint farm.

    -   Select the option to use the existing managed account (*Contoso\SharePoint*), and click **Next**.

    -   On the **Creating a Site Collection** window, click **Skip**.  Then click **Finish**.

7.  After the wizards complete, use **PowerShell** to create a **SharePoint Foundation 2013 Web Application** to host the **MIM Portal**.

    > [!NOTE]
    > Initially, SSL will not be configured. Be sure to configure SSL or equivalent before enabling access to this portal.

    1.  Launch  **SharePoint 2013 Management Shell** and run the following PowerShell script:

        ```
        $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
        New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"
          -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
        ```

    2.  Note that a warning message will appear that Windows Classic authentication method is being used, and it may take several minutes for the final command to return.  When completed, the output will indicate the URL of the new portal.  Keep the **SharePoint 2013 Management Shell** window open as it will be needed in a subsequent task.

8.  Create a **SharePoint Site Collection** associated with that web application to host the **MIM Portal**.

    1.  Launch  SharePoint 2013 Management Shell and run the following PowerShell script:

        ```
        $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
        $w = Get-SPWebApplication http://corpidm.domain.local:82 
        New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator 
         -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin 
        $s = SpSite($w.Url)
        $s.AllowSelfServiceUpgrade = $false
        $s.CompatibilityLevel
        ```

    2.  Verify that the result of the *CompatibilityLevel* variable is “14”.  ([See http://technet.microsoft.com/library/jj863242(v=ws.10).aspx](http://technet.microsoft.com/library/jj863242%28v=ws.10%29.aspx) for more information). If the result is “15”, then the site collection was not created for the 2010 experience version; delete the site collection and recreate it.

9. Disable **SharePoint Server-Side Viewstate**, and the SharePoint task "Health Analysis Job (Hourly, Microsoft SharePoint Foundation Timer, All Servers)", by running the following PowerShell commands in the **SharePoint 2013 Management Shell**:

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

10. On your identity management server, Open a new web browser tab, navigate to **http://localhost:82/** and login as *contoso\Administrator*.  An empty SharePoint site named *MIM Portal* will be shown.

    ![](../Image/MIM_DeploySP1.png)

11. Copy the URL, then in **Internet Explorer**, open **Internet Options**, change to the **Security tab**, select **Local intranet**, click on **Sites**, then click on **Advanced** and add the copied URL to the list. Click **OK** twice.

    ![](../Image/MIM_DeploySP2.png)

12. Open **Administrative**  , then **Tools**, **Services**, locate the SharePoint Administration service, and start it, if it is not already running.

## Optional: Deploy Microsoft Exchange Server
If you would like to configure MIM to send and receive email or provision mailboxes, then it is necessary to have Exchange present in the environment. If you do not have Exchange already deployed, then you can install a trial version for evaluation purposes.

-   Download and install Microsoft Office 2010 Filter Packs - Version 2.0 + Microsoft Office 2010 Filter Packs - Version 2.0 SP1

    -   [MS Office10 FP2.0](http://www.microsoft.com/en-us/download/details.aspx?id=17062)

    -   [MS Office10 FP2.0 SP1](http://www.microsoft.com/en-us/download/details.aspx?id=26604)

-   Download and install [Microsoft Unified Communications Managed API 4.0, Core Runtime 64-bit](http://www.microsoft.com/en-us/download/details.aspx?id=34992)

-   Download and install [MS Exchange Server 2013 180-day Trial version](http://www.microsoft.com/en-us/evalcenter/evaluate-exchange-server-2013)

