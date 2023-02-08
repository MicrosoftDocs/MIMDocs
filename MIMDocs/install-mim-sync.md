---
# required metadata

title: Install the Microsoft Identity Manager Sync Service | Microsoft Docs
description: Installing and configuring the MIM Synchronization Service.
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 01/27/2023
ms.topic: conceptual
ms.prod: microsoft-identity-manager

ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: markwahl-msft
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Install MIM 2016: MIM Synchronization Service

> [!div class="step-by-step"]
> > [« SharePoint](prepare-server-sharepoint.md)
> [MIM Service and Portal »](install-mim-service-portal.md)
 
> [!NOTE]
> This walkthrough uses sample names and values from a company called Contoso. Replace these with your own. For example:
> - Domain controller name - **corpdc**
> - Domain name - **contoso**
> - MIM Service Server name - **corpservice**
> - MIM Sync Server name - **corpsync**
> - SQL Server name - **corpsql**
> - Password - <strong>Pass@word1</strong>

To install Microsoft Identity Manager 2016 components, first set up the installation package.

1. Sign in as *contoso\miminstall* to the server you are using for identity management synchronization server **corpsync**.

2. Unpack the MIM installation package or mount the MIM image DVD.  If you do not have this DVD, see [Microsoft Identity Manager licensing and downloads](microsoft-identity-manager-licensing.md).

## Install MIM 2016 SP1 or later Synchronization Service

1. In the unpacked MIM installation folder, navigate to the **Synchronization Service** folder.

2. Run the **MIM Synchronization Service installer**. Follow the guidelines of the installer and complete the installation.

3. In the welcome screen – click **Next**.

    ![MIM installer wizard welcome image](media/install-mim-sync/MIM_Install1.png)

4. Review the license terms and click **Next** to accept them.

5. On the **Custom Setup** screen click **Next**.

    ![Custom Setup image](media/install-mim-sync/MIM_Install2.png)

6. In the Sync Service database configuration screen, select:

   1.  The SQL Server is located on: **A Remote machine** called **corpsql.contoso.com**.

   2.  The SQL Server instance is: **The default instance**

   ![Database connection image](media/install-mim-sync/MIM_Install3.png)

    3. *MIM 2016 SP2 and later*: Configure the MIM Synchronization Service Database name

7. Configure the Sync Service Account according to the account you created earlier:

   1. Service account: *MIMSync*

   2. Password: <em>Pass@word1</em>

   3. Service Account Domain or local computer name: *contoso*

    >[!NOTE]
    >MIM 2016 SP2 and later: for Group Managed Service Accounts, ensure the **$** character is at the end of the Service Account Name, e.g. MIMSync$, and leave the Password field empty.

    ![Service account image](media/install-mim-sync/MIM_Install4.png)

8. Provide MIM Sync Service installer with the relevant security groups:

   1. Administrator = *contoso\MIMSyncAdmins*

   2. Operator= *contoso\MIMSyncOperators*

   3. Joiner = *contoso\MIMSyncJoiners*

   4. Connector Browse = *contoso\MIMSyncBrowse*

   5. WMI Password Management= *contoso\MIMSyncPasswordReset*

   ![Security groups image](media/install-mim-sync/MIM_Install5.png)

9. In the security settings screen, check **Enable firewall rules for inbound RPC communications**, and click **Next**.

10. Click **Install** to begin the installation of MIM Sync Service.

    1. A warning concerning the MIM Sync service account may appear – click **OK**.

    2. MIM Sync Service will install.

    3. A notice on creating a backup for the encryption key appears – click **OK**, then select a folder to store the encryption key backup.

    ![MIM Sync backup encryption key notice image](media/MIM-Install7.png)

    4. When the installer successfully completes the installation, click **Finish**.

    5. You need to sign out and sign in for the group membership changes to take effect. Click **Yes** to sign out.

> [!div class="step-by-step"]  
> > [« SharePoint](prepare-server-sharepoint.md)
> [MIM Service and Portal »](install-mim-service-portal.md)
