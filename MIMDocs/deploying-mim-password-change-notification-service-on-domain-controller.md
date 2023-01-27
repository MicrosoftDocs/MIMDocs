---
# required metadata

title: Deploy the password change notification service | Microsoft Docs
description: Get the steps to install and configure the MIM Password Change Notification Service on your domain controller.
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/27/2023
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Deploy the MIM Password Change Notification Service on a domain controller

## Install the Password Change Notification Service
The Password Change Notification Service (PCNS) is a service that you install on the domain controllers that enables synchronization of passwords by MIM to other systems, such as another vendor's directory server. For password synchronization, install the PCNS on each domain controller server.

1.  Login as a domain administrator to a Server running on Windows Server with the role of an Active Directory Domain Services.

2.  Copy the Password Change Notification Service setup folder to the computer.

3.  Locate the *Password Change Notification Service.msi* file, right click on it, and create a shortcut.

4.  Locate the shortcut file, right click, and bring up its **Properties**.

5.  In the Target field add the preamble *msiexec.exe /i* before the path to the msi file, and the suffix *SCHEMAONLY=TRUE* after the msi path. For example, if the setup folder is *C:\PCNS* the command to run would look like this: (all in one line).

    ```
    msiexec.exe /i "C:\PCNS\x64\Password Change Notification Service.msi" SCHEMAONLY=TRUE
    ```

6.  Save changes to the shortcut file.

7.  Run the shortcut file to start the PCNS installation in schema extension mode. When the following screen appears, click **Next**.

8.  You will be notified that Setup will now update the Active Directory schema for the Password Change Notification Service. Click **OK** to proceed with the schema update.

9. When the Schema extension process completes, and the following screen appears, click **Finish**.

10. Run the *Password Change Notification Service.msi* file again – this time directly (no run string is needed).  When the following screen appears, click **Next**.

11. Accept the license agreement and click **Next**.

12. Click to begin the installation.

13. When the installation completes successfully screen appears, click **Finish**.

14. Restart your computer for the configuration changes made to MIM Password Change Notification Service to take effect. You can do it by clicking **Yes** in the pop-up window that appears, or you can restart later.

## Configuring the Password Change Notification Service
Once reconnected to the DC server as a domain administrator, go to *C:\Program Files\Microsoft Password Change Notification.* Run *pcnscfg.exe*.
