---
# required metadata

title: Deploy PAM step 7 – user access | Microsoft Docs
description: As the final step, grant a privileged user access to demonstrate that your Privileged Access Management deployment was successful.
author: henrymbuguakiarie
ms.author: henrymbuguakiarie
ms.date: 04/08/2025
ms.topic: article
ms.service: microsoft-identity-manager

ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
---
# Step 7 – Elevate a user’s access

> [!div class="step-by-step"]
> [« Step 6 ](step-6-transition-group-to-pam.md)


This step demonstrates that a user can request access to a role via MIM.

## Verify that access to resource is restricted

Without elevated privileges, Jen's account won't be able to access the privileged resource in the CORP forest.

1. Have Jen sign out of all computers to remove any cached open connections.
2. Sign in to PRIVWKSTN.
3. Open a DOS command prompt.
4. Type the command that will require the user to have a security group membership.  For example, if the security group is protecting the ability to use a file share `corpfs` on the **CORPDC** computer, type `dir \\corpdc\corpfs`. The error message **Access is denied** should appear.
5. Leave the command prompt window open.

## Request privileged access from MIM

> [!NOTE]
> It is recommended that the workstation be a privileged workstation(PAW).  For more information, see the [securing devices](/security/compass/privileged-access-devices) guidance.

1. On PRIVWKSTN, log on as `PRIV\priv.jen`.
2. Launch PowerShell.
3. Type the following command.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. When prompted, type the password for the `PRIV.Jen` account. A new command prompt window will appear.
3. When the PowerShell window appears, type the following commands.

    > [!NOTE]
    > After you run these commands, all the following steps are time-sensitive.

    ```PowerShell
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. After that completes, close the PowerShell window.
5. In the command window, type the following command:

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Type the password for the `PRIV.Jen` account. A new command prompt window will appear.

7. Validate the elevated access in the newly opened window has provided the user with new group memberships. type the following command.

    ```cmd
    whoami /groups
    ```

8. Next, type the command which earlier in the step had been shown to have been blocked due to lack of access. For example, if the resource was a file share `corpfs`, type the following command.

    ```cmd
    dir \\corpdc\corpfs
    ```

    If the dir command fails with the error message **Access is denied**, recheck the trust relationship.


## Summary

Now that you've completed this walkthrough, you've demonstrated a Privileged Access Management scenario. In this scenario, user privileges were elevated for a limited amount of time, allowing the user to access protected resources with a separate privileged account. As soon as the elevation session expires, the privileged account can no longer access the protected resource. Next, once you migrate access rights to the Privileged Access Management system, access that was permanently available to the original user account, will only be possible to special accounts upon request. As a result, group memberships for highly privileged groups are available only for limited periods of time.

> [!div class="step-by-step"]
> [« Step 6 ](step-6-transition-group-to-pam.md)
