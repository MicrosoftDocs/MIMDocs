---
# required metadata

title: Step 7 – Elevate a user’s access | Microsoft Identity Manager
description:
keywords:
author: kgremban
manager: femila
ms.date: 06/16/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Step 7 – Elevate a user’s access

>[!div class="step-by-step"]
[« Step 6 ](step-6-transition-group-to-pam.md)


This step demonstrates that a user can request access to a role via MIM.

## Verify that Jen cannot access the privileged resource
Without elevated privileges, Jen cannot access the privileged resource in the CORP forest.

1. Sign out of CORPWKSTN to remove any cached open connections.
2. Sign in to CORPWKSTN as *CONTOSO\Jen* and switch to the **Desktop** view.
3. Open a DOS command prompt.
4. Type the command `dir \\corpwkstn\corpfs`. The error message **Access is denied** should appear.
5. Leave the command prompt window open.

## Request privileged access from MIM.
1. On CORPWKSTN, still as CONTOSO\Jen, type the following command.

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. When prompted, type the password for the PRIV.Jen account. A new command prompt window will appear.
3. When the PowerShell window appears, type the following commands.

    > [!NOTE] 
    > After you run these commands, all the following steps are time-sensitive.

    ```
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. After that completes, close the PowerShell window.
5. In the DOS command window, type the following command

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Type the password for the PRIV.Jen account. A new command prompt window will appear.

## Validate the elevated access.
In the newly opened window, type the following commands.

```
whoami /groups
dir \\corpwkstn\corpfs
```

If the dir command fails with the error message **Access is denied**, re-check the trust relationship.

## Activate the privileged role
Activate by requesting privileged access via the PAM sample portal.

1. On CORPWKSTN, make sure that you are signed in as CORP\Jen.
2. Type the following command in a DOS command window.

    ```
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. When prompted, type the password for the PRIV.Jen account. A new web browser window will appear.
4. Navigate to http://pamsrv.priv.contoso.local:8090 and ensure that a web page from the sample portal is visible.
5. In Internet Explorer, select **Tools** > **Internet Options** and click the **Security** tab.
6. Click on the **Local intranet zone** > **Sites** > **Advanced** then add the website to the zone.
7. Close the **Internet Options** dialogs.
8. On the left tab, click **Activate**. Select the **PAM role** and then click **Activate**.

> [!Note] 
> In this environment, you can also learn how to develop applications which use the PAM REST API, described in the [Privileged Access Management REST API Reference](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference.md).

## Summary
Once you have completed the steps in this walkthrough, you will have demonstrated a Privileged Access Management scenario, in which user privileges are elevated for a limited amount of time, allowing the user to access protected resources with a separate privileged account. As soon as the elevation session expires, the privileged account can no longer access the protected resource. The decision of which security groups represent privileged roles is coordinated by the PAM administrator. Once access rights are migrated to the Privileged Access Management system, access that was previously possible with the original user account is now made possible only by signing in with a special privileged account, and made available upon request. As a result, group memberships for highly privileged groups are effective for a limited amount of time.

>[!div class="step-by-step"]
[« Step 6 ](step-6-transition-group-to-pam.md)
