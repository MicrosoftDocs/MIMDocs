---
title: Step 7 – Elevate a user’s access
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
author: Kgremban
---
# Step 7 – Elevate a user’s access
This step will demonstrate that a user can request access to a role via MIM.

## Verify that Jen cannot access the privileged resource
Verify that Jen cannot access the privileged resource in the CORP forest using her account CONTOSO\Jen.

1. On *CORPWKSTN*, sign out. (This will remove any cached open connections.)
2. Sign into *CORPWKSTN* as *CONTOSO\Jen* and switch to the **Desktop** view.
3. Open a **DOS** command prompt.
4. Type the command `dir \\corpwkstn\corpfs`. The error message “Access is denied.” should appear.
5. Leave the command prompt window open.

## Request privileged access from MIM.
1. On *CORPWKSTN*, ensure that you are logged in as *CONTOSO\Jen* and a **DOS** command window is open.
2. Type the following command.

    `runas /user:Priv.Jen@priv.contoso.local powershell`

3. When prompted, type the password for the *PRIV.Jen* account. A new command prompt window will appear.
4. When the PowerShell window appears, change to that window and type the following commands. **Note**: All subsequent interactions are time-sensitive.

```
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
```
<br/>
5. After that completes, close the newly-opened **PowerShell** window.
6. In the DOS command window, type the following command

    `runas /user:Priv.Jen@priv.contoso.local powershell`

7. Type the password for the *PRIV.Jen* account. A new command prompt window will appear.

## Validate the elevated access.
In the newly opened window, type the following commands.

```
    whoami /groups
    dir \\corpwkstn\corpfs
```
<br/>
If the dir command fails with the error message "Access is denied", re-check the trust relationship.

## (Optional) Activate
Activate by requesting privileged access via the PAM sample portal.

1. On *CORPWKSTN*, ensure that you are logged in as *CORP\Jen* and a DOS command window is open.
2. Type the following command.

    `runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"`

3. When prompted, type the password for the *PRIV.Jen* account. A new web browser window will appear.
4. Navigate to *http://pamsrv.priv.contoso.local:8090* and ensure that a web page from the sample portal is visible.
5. In Internet Explorer, select **Tools** \> **Internet Options** and click the **Security** tab.
6. Click on the **Local intranet zone** \> **Sites** \> **Advanced** then add the website to the zone.
7. Close the **Internet Options** dialogs.
8. On the left tab, click **Activate**. Select the *PAM role* and then click **Activate**.

**Note**: In this environment, you can also learn how to develop applications which use the PAM REST API, described in the [Privileged Access Management REST API Reference](https://msdn.microsoft.com/en-us/library/mt228271.aspx) to activate.

## Summary
Once you have completed the steps in this test lab guide, you will have demonstrated a privileged access management scenario, in which user privileges are elevated for a limited amount of time, allowing the user to access protected resources with a separate privileged account. As soon as the elevation session expires, the privileged account can no longer access the protected resource. The decision which security groups represent privileged roles is coordinated by the *PAM administrator*. Once access rights are migrated to the privileged access management system, access that was previously made possible with the original user account is now made possible only by logging in with a special privileged account, and made available upon request. As a result, group memberships for highly privileged groups are effective for a limited amount of time.
