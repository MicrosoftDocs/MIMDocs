---
title: Installing MIM 2016&#58; MIM Synchronization Service | Microsoft Identity Manager
ms.custom:
  - Identity Management
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - security
ms.tgt_pltfrm: na
ms.topic: get-started-article
author: kgremban
---
# Installing MIM 2016: MIM Synchronization Service

>[!div class="step-by-step"]
[Previous](https://docsmsftstage.azurewebsites.net/MIM/DeployUse/prepare-server-exchange.html)
**Preparing an identity management server: Exchange (optional)**

> [!NOTE]
> In all the examples below, **mimservername** represents the name of your domain controller, **contoso** represents your domain name, and **Pass@word1** represents an example password.

To install Microsoft Identity Manager 2016 components, first:

1. Sign in as *contoso\Administrator* to the CORPIDM server you are using for identity management.

2. Unpack the MIM installation package or mount the MIM image DVD.

## Install MIM 2016 Synchronization (Sync) Service

1. In the unpacked MIM installation folder, navigate to the **Synchronization Service** folder.

2. Run the **MIM Synchronization Service installer**. Follow the guidelines of the installer and complete the installation.

3. In the welcome screen – click **Next**.

    ![MIM installer wizard welcome image](media/MIM-Install1.png)

4. Review the license terms and if you accept them, click **Next**.

5. On the **Custom Setup** screen click **Next**.

    ![Custom Setup image](media/MIM-Install2.png)

6.  In the Sync database configuration screen, select:

    1.  The SQL Server is located on: **This computer**.

    2.  The SQL Server instance is: **The default instance**.

    ![Database connection image](media/MIM-Install3.png)

7.  Configure the Sync Service Account according to the account you created earlier:

    1.  Service account: *MIMSync*

    2.  Password: *Pass@word1*

    3.  Service Account Domain or local computer name: *contoso*

    ![Service account image](media/MIM-Install4.png)

8.  Provide MIM Sync installer with the relevant security groups:

    1.  Administrator = *contoso\MIMSyncAdmins*

    2.  Operator= *contoso\MIMSyncOperators*

    3.  Joiner = *contoso\MIMSyncJoiners*

    4.  Connector Browse = *contoso\MIMSyncBrowse*

    5.  WMI Password Management= *contoso\MIMSyncPasswordReset*

    ![Security groups image](media/MIM-Install5.png)

9. In the security settings screen, check **Enable firewall rules for inbound RPC communications**, and click **Next**.

10. Click **Install** to begin the installation of MIM Sync.

    1.  A warning concerning the MIM Sync service account may appear – click **OK**.

    2.  MIM Sync will now be installed.

        ![MIM Sync installation status image](media/MIM-Install6.png)

    3.  A notice on creating a backup for the encryption key will be shown – click **OK**, then select a folder to store the encryption key backup.

        ![MIM Sync backup encryption key notice image](media/MIM-Install7.png)

    4.  When the installer successfully completes the installation, click **Finish**.

        ![MIM Sync installation success image](media/MIM-Install8.png)

    5.  You will be prompted to sign out and sign in for the group membership changes to take effect. Click **Yes** to sign out.

>[!div class="step-by-step"]  
[Next](https://docsmsftstage.azurewebsites.net/MIM/DeployUse/mim-install-service-portal.html)
**Installing MIM 2016: MIM Service and Portal**
