---
# required metadata

title: Configure SharePoint | Microsoft Identity Manager
description: Install and configure SharePoint Foundation so that it can host the MIM Portal page.
keywords:
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Set up an identity management server: SharePoint

>[!div class="step-by-step"]
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> This walkthrough uses sample names and values from a company called Contoso. Replace these with your own. For example:
> - Domain controller name - **mimservername**
> - Domain name - **contoso**
> - Password - **Pass@word1**


## Install **SharePoint Foundation 2013 with SP1**

> [!NOTE]
> The installer requires an Internet connection to download its prerequisites. If the computer is on a virtual network which does not provide Internet connectivity, add an additional network interface to the computer that provides a connection to the Internet. This can be disabled after installation is completed.

Follow these steps to install SharePoint Foundation 2013 SP1. After you finish installation, the server will restart.

1.  Launch **PowerShell** as a domain administrator.

    -   Change to the directory where SharePoint was unpacked.

    -   Type the following command.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  After **SharePoint** prerequisites are installed, install **SharePoint Foundation 2013 with SP1** by typing the following command:

    ```
    .\setup.exe
    ```

3.  Select the complete server type.

4.  After the install completes, run the wizard.

## Run the wizard to configure SharePoint

Follow the steps lined out in the **SharePoint Products Configuration Wizard** to configure SharePoint to work with MIM.

1. On the **Connect to a server farm** tab, change to create a new server farm.

2. Specify this server as the database server for the configuration database, and *Contoso\SharePoint* as the database access account for SharePoint to use.

3. Create a password for the farm security passphrase.

4. When the configuration wizard completes configuration task 10 of 10, click Finish and a web browser will open.

5. In the Internet Explorer popup, authenticate as *Contoso\Administrator* (or the equivalent domain administrator account) to proceed.

6. Start the wizard (within the web app) to configure the SharePoint farm.

7. Select the option to use the existing managed account (*Contoso\SharePoint*), and click **Next**.

8. On the **Creating a Site Collection** window, click **Skip**.  Then click **Finish**.

## Prepare SharePoint to host the MIM Portal

> [!NOTE]
> Initially, SSL will not be configured. Be sure to configure SSL or equivalent before enabling access to this portal.

1. Launch  **SharePoint 2013 Management Shell** and run the following PowerShell script to create a **SharePoint Foundation 2013 Web Application**.

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"
    -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
    ```

    > [!NOTE]
    > A warning message will appear saying that Windows Classic authentication method is being used, and it may take several minutes for the final command to return. When completed, the output will indicate the URL of the new portal. Keep the **SharePoint 2013 Management Shell** window open to reference later.

2. Launch  SharePoint 2013 Management Shell and run the following PowerShell script to create a **SharePoint Site Collection** associated with that web application.

  ```
  $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
  $w = Get-SPWebApplication http://corpidm.contoso.local:82
  New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
  -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
  $s = SpSite($w.Url)
  $s.AllowSelfServiceUpgrade = $false
  $s.CompatibilityLevel
  ```

  > [!NOTE]
  > Verify that the result of the *CompatibilityLevel* variable is “14”. If the result is “15”, then the site collection was not created for the 2010 experience version; delete the site collection and recreate it.

3. Disable **SharePoint Server-Side Viewstate** and the SharePoint task "Health Analysis Job (Hourly, Microsoft SharePoint Foundation Timer, All Servers)" by running the following PowerShell commands in the **SharePoint 2013 Management Shell**:

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. On your identity management server, open a new web browser tab, navigate to http://localhost:82/ and login as *contoso\Administrator*.  An empty SharePoint site named *MIM Portal* will be shown.

    ![MIM Portal at http://localhost:82/ image](media/MIM-DeploySP1.png)

5. Copy the URL, then in Internet Explorer, open **Internet Options**, change to the **Security tab**, select **Local intranet**, and click **Sites**.

    ![Internet Options image](media/MIM-DeploySP2.png)

6. In the **Local intranet** window, click on **Advanced** and paste the copied URL in the **Add this website to the zone** text box. Click **Add** then close the windows.

7. Open the **Administrative Tools** program, navigate to **Services**, locate the SharePoint Administration service, and start it if it is not already running.

>[!div class="step-by-step"]  
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)
