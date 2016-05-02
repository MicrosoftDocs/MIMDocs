---
# required metadata

title: Set up an identity management server&#58; Windows Server 2012 R2 | Microsoft Identity Manager
description: Get the steps and minimum requirements to prepare Windows Server 2012 RS to work with MIM 2016.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Set up an identity management server: Windows Server 2012 R2

>[!div class="step-by-step"]
[« Preparing a domain](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)

> [!NOTE]
> This walkthrough uses sample names and values from a company called Contoso. Replace these with your own. For example:
> - Domain controller name - **mimservername**
> - Domain name - **contoso**
> - Password - **Pass@word1**

## Join Windows Server 2012 R2 to your domain

Start with a Windows Server 2012 R2 machine, with a minimum of 8GB of RAM. When installing specify "Windows Server 2012 R2 Standard (Server with a GUI) x64" edition.

1. Log into the new computer as its administrator.

2. Using the Control Panel, give the computer a static IP address on the network. Configure that network interface to send DNS queries to the IP address of the domain controller in the previous step, and set the computer name to **CORPIDM**.  This will require a server restart.

3. Open the Control Panel and join the computer to the domain that you configured in the last step, *contoso.local*.  This includes providing the username and credentials of a domain administrator such as *Contoso\Administrator*.  After the welcome message appears, close the dialog box and restart this server again.

4. Sign in to the computer *CorpIDM* as a domain administrator such as *Contoso\Administrator*.

5. Launch a PowerShell window as administrator and type the following command to update the computer with the group policy settings.

    ```
    gpupdate /force /target:computer
    ```

    After no more than a minute, it will complete with the message "Computer Policy update has completed successfully."

6. Add the **Web Server (IIS)** and **Application Server** roles, the **.NET Framework** 3.5, 4.0, and 4.5 features, and the **Active Directory module for Windows PowerShell**.

    ![PowerShell features image](media/MIM-DeployWS2.png)

7. In PowerShell, type the following commands. Note that it may be necessary to specify a different location for the source files for **.NET Framework** 3.5 features. These features are typically not present when Windows Server installs, but are available in the side-by-side (SxS) folder on the OS install disk sources folder, e.g., “*d:\Sources\SxS\*”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## Configure the server security policy

Set up the server security policy to allow the newly-created accounts to run as services.

1. Launch the Local Security Policy program

2. Navigate to **Local Policies > User Rights Assignment**.

3. On the details pane, right click on **Log on as a service**, and select **Properties**.

    ![Local Security Policy image](media/MIM-DeployWS3.png)

4. Click **Add User or Group**, and in the text box type `contoso\mimsync; contoso\mimma; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\mimsspr`, click **Check Names**, and click **OK**.

5. Click **OK** to close the **Log on as a service Properties** window.

6.  On the details pane, right click on **Deny access to this computer from the network**, and select **Properties**.

7. Click **Add User or Group**, and in the text box type `contoso\MIMSync; contoso\MIMService` and click **OK**.

8. Click **OK** to close the **Deny access to this computer from the network Properties** window.

9. On the details pane, right click on **Deny log on locally**, and select **Properties**.

10. Click **Add User or Group**, and in the text box type `contoso\MIMSync; contoso\MIMService` and click **OK**.

11. Click **OK** to close the **Deny log on locally Properties** window.

12. Close the Local Security Policy window.


## Change the IIS Windows Authentication mode

1.  Open a PowerShell window.

2.  Stop IIS with the command *iisreset /STOP*

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

>[!div class="step-by-step"]  
[« Preparing a domain](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)
