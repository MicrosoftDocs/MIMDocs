---
# required metadata

title: Raise the bastion forest functional level for Identity Manager to use Active Directory PAM features| Microsoft Docs
description: Raise a privileged access management deployment that started with Windows Server 2012 R2 functional level to the Windows Server 2016 functional level.
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 09/14/2023
ms.topic: article
ms.prod: microsoft-identity-manager

---
# Raise the bastion forest functional level to use Active Directory PAM features

Microsoft Identity Manager privileged access management (PAM) in Identity Manager 2016 was originally designed to operate with either the Windows Server 2012 R2 or Windows Server 2016 Active Directory functional level of the bastion forest.

For Identity Manager to use Windows Server 2012 R2, it contained a "just in time" evaluation engine in the Identity Manager PAM component service that could remove members from groups. With Windows Server 2016, PAM features of time-limited group memberships and shadow principal groups are built into Windows Server Active Directory. For more information, see [What's new in Active Directory Domain Services for Windows Server 2016](/windows-server/identity/whats-new-active-directory-domain-services).

Windows Server 2016 and later versions offer these features and more security benefits. If you have an existing deployment of Identity Manager PAM at the 2012 functional level, plan to update Identity Manager and Windows Server so that you can raise the functional level. The use of Identity Manager with Windows Server 2012 R2 as the PRIV forest functional level is now deprecated. We recommend that you set the CORP forests at the Windows Server 2016 functional level for the PAM scenario for any newly created or non-low RID groups. This step isn't required.

For an existing deployment, raising the functional level of the bastion forest requires more configuration steps, both in Active Directory and Identity Manager. The following steps ensure that time-limited memberships are enabled and Identity Manager recognizes the new features of that functional level.

## Upgrade Windows Server and Identity Manager software

Before you raise the functional level, ensure that the domain controllers, member servers, and Identity Manager meet the minimum version requirements:

* All domain controllers in the bastion environment for the PRIV forest must be Windows Server 2016 or later.
* All member servers hosting Identity Manager for PAM must be Windows Server 2016 or later, as needed for the Remote Server Administration Tools.
* The Identity Manager software must be Identity Manager 2016 version 4.6.607.0 from February 2022 or a later hotfix. For more information on the latest hotfixes, see [Microsoft Identity Manager version history](../reference/version-history.md).

## Check if you need to raise the functional level

1. Make sure you're signed in to the server that hosts Identity Manager as an Identity Manager administrator.
1. Start PowerShell.
1. Enter the following command.

    ```powershell
    (Get-PAMConfiguration).ForestFunctionality
    ```

    If the output is `DS_BEHAVIOR_WIN2012R2`, you might need to raise the functional level and complete the Identity Manager and PAM configuration. You'll follow the steps in the rest of this article. If the output is `DS_BEHAVIOR_WIN2015`, you don't need to raise the functional level.

## Raise the PRIV forest functional level

1. To perform this step, you must be a member of the Enterprise Admins group in the PRIV forest.

1. Open Active Directory Domains and Trusts.

1. In the console tree, right-click **Active Directory Domains and Trusts**. Then select **Raise Forest Functional Level**.

1. If the current functional level shown is Windows Server 2016 or a later functional level, you don't need to raise the functional level of the forest. But you might need to make more Active Directory or Identity Manager changes.

1. If the functional level is earlier than Windows Server 2016, in **Select an available forest functional level**, select the value **Windows Server 2016**. Then select **Raise**.

   For more information on how to raise the functional level, or if an error occurs, see [Raise Active Directory domain and forest functional levels](/troubleshoot/windows-server/identity/raise-active-directory-domain-forest-functional-levels).

1. In the console tree, select the domain. Then select **Raise Domain Functional Level**.

1. If the current functional level shown is earlier than Windows Server 2016, in **Select an available forest functional level**, select the value **Windows Server 2016**. Then select **Raise**.

1. Close Active Directory Domains and Trusts.

## Update the PRIV domain configuration

Next, authorize the Identity Manager administrators and Identity Manager accounts to create and update shadow principals.

1. Ensure the PAM features in Windows Server 2016 Active Directory are present and enabled in the PRIV forest. Open a PowerShell window as an administrator, and enter the following commands:

   ```
   $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
   Enable-ADOptionalFeature $of -scope ForestOrConfigurationSet -target "priv.contoso.local"
   ```
1. Acknowledge the change.

1. Enter **ADSIEdit**.

1. Open the **Actions** menu and select **Connect To**. On the **Connection point** setting, change the naming context from **Default naming context** to **Configuration**. Select **OK**.

1. On the left side of the window under **ADSI Edit**, select the **Configuration** node to see **CN=Configuration,DC=priv,....**. Select **CN=Configuration**, and then select **CN=Services**.

1. Right-click **CN=Shadow Principal Configuration** and select **Properties**. When the properties dialog appears, select the **Security** tab.

1. Select **Add**. Specify the accounts **MIMService**, **MIMMonitor**, **MIMComponent**, and any other Identity Manager administrators who will later be performing New-PAMGroup to create more PAM groups. For each user, in the allowed permissions list, add **Write**, **Create all child objects**, and **Delete all child objects**. Add the permissions.

1. Select **Advanced** to change to the **Advanced Security** settings. Select the line that allows **MIMService** access, and select **Edit**. Change the **Applies to** setting to **to this object and all descendant objects**. Update this permission setting and close the **Security** dialog. Repeat these steps for the other accounts.

1. Close **ADSI Edit**.

1. Next, authorize the Identity Manager administrators to create and update authentication policy. Open an elevated command prompt and enter the following commands. Substitute the name of your Identity Manager administrator account for `mimadmin` in each of the four lines:

    ```
    dsacls "CN=AuthN Policies,CN=AuthN Policy Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

    dsacls "CN=AuthN Policies,CN=AuthN Policy Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g mimadmin:CCDC;msDS-AuthNPolicy

    dsacls "CN=AuthN Silos,CN=AuthN Policy Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

    dsacls "CN=AuthN Silos,CN=AuthN Policy Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g mimadmin:CCDC;msDS-AuthNPolicySilo
    ```

1. Restart the PRIVDC server and other domain controllers so that the changes take effect.

## Update the Identity Manager PAM configuration

1. Stop accepting new PAM requests, and wait for existing PAM requests to finish.
1. Make sure you're signed in to the server that hosts Identity Manager as an Identity Manager administrator.
1. Start PowerShell.
1. Enter the following command:

    ```powershell
    Set-PAMConfiguration -ForestFunctionality DS_BEHAVIOR_2015
    ```

1. Restart the server where Identity Manager is hosted.

## Validate that Active Directory shadow principals are being created

After the functional levels are raised, the PAM feature is enabled in Active Directory and the Identity Manager configuration changes. Identity Manager continues to use the existing PRIV forest groups that were created by using SID history, or that are Priv-only groups. In those groups, Identity Manager uses the Active Directory TTL membership feature to time-limit the membership. For any new groups, the `New-PAMGroup` cmdlet creates new shadow security principal groups rather than using SID history.

1. Create a new security group in a CORP forest domain. For example, the group could be named **G2**.
1. Make sure you're signed in to the server that hosts Identity Manager as an Identity Manager administrator.
1. Start PowerShell.
1. Enter the following command, referring to the group that was created:

    ```powershell
    New-PAMGroup -SourceDomain "CORP" -SourceGroupName "G2"
    ```

1. After the command finishes, check that in the output object the **Source Account SID** and **Priv Account SID** match. The resulting shadow principal object will have been created in the PRIV domain under `CN=Shadow Principal Configuration,CN=Services,CN=Configuration`.

## Next steps

[Elevate a user's access](step-7-elevate-user-access.md)
