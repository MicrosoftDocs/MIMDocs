---
# required metadata

title: Raise the bastion forest functional level | Microsoft Docs
description: Raise a Privileged Access Management deployment that started with Windows Server 2012 R2 functional level to the Windows Server 2016 functional level.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/27/2021
ms.topic: article
ms.prod: microsoft-identity-manager

---
# Raise the bastion forest functional level

MIM PAM in MIM 2016 was originally designed to operate with either the Windows Server 2012 R2 or Windows Server 2016 functional level of the bastion forest.  For MIM to use Windows Server 2012 R2, it contained a "just in time" evaluation engine in the *MIM PAM component* service, that could remove members from groups.  With Windows Server 2016, time-limited group memberships are built into Windows Server AD, as described in [What's new in Active Directory Domain Services for Windows Server 2016](/windows-server/identity/whats-new-active-directory-domain-services). As Windows Server 2016 and later versions offers this and additional security benefits, if you have an existing deployment of MIM PAM with the 2012 functional level, you should plan to upgrade MIM and Windows Server of the bastion forest and raise the functional level.  The use of MIM with Windows Server 2012 R2 as the **PRIV** forest functional level is now deprecated. (The use of Windows Server 2016 and the Windows Server 2016 functional level in the **CORP** managed forests is recommended, but not required, for the PAM scenario.)

For an existing deployment, raising the functional level of the bastion forest requires additional configuration steps, both in Active Directory and MIM. The steps listed below will ensure that time-limited memberships are enabled and MIM recognizes the new features of that functional level.

## Step 1 - Upgrade Windows Server and MIM software

Before raising the functional level, ensure that the domain controllers, member servers and MIM meet the minimum version requirements.

* All domain controllers in the bastion environment for the `PRIV` forest must be Windows Server 2016 or later.
* All member servers hosting MIM Service for PAM must be Windows Server 2016 or later.
* The MIM Service software must be MIM 2016 version 4.6.607.0 (from February 2022) or a later hotfix.  See [Microsoft Identity Manager version history](../reference/version-history.md) for more information on the latest hotfixes.

## Step 2- check if you need to raise the functional level

1. Make sure you're signed in into the server hosting MIM Service as a MIM administrator.
2. Launch PowerShell.
3. Type the following command.

    ```powershell
    (Get-PAMConfiguration).ForestFunctionality
    ```

    If the output is `DS_BEHAVIOR_WIN2012R2`, then you may need to raise the functional level and complete the MIM and PAM configuration, as outlined in the rest of this article.  If the output is `DS_BEHAVIOR_WIN2015`, then you don't need to raise the functional level.

## Step 3 - Raise the PRIV forest functional level

1. To perform this step, you must be a member of the Enterprise Admins group in the *PRIV* forest.

2. Open Active Directory Domains and Trusts.

3. In the console tree, right-click **Active Directory Domains and Trusts**, and then click **Raise Forest Functional Level**.

4. If the current functional level shown is **Windows Server 2016** or **Windows Server 2019**, then you don't need to raise the functional level, but may need to make further AD or MIM changes.  Close Active Directory Domains and Trusts and continue at the next step.

5. In **Select an available forest functional level**, select the value **Windows Server 2016** , and then click **Raise**.

For more information on raising the functional level, or if an error occurs, see [how to raise Active Directory domain and forest functional levels](/troubleshoot/windows-server/identity/raise-active-directory-domain-forest-functional-levels).

6. Close Active Directory Domains and Trusts.

## Step 4 - Update the PRIV domain configuration

Next, authorize the MIM administrators and MIM Service account to create and update shadow principals.

1. Enable the Privileged Access Management features in Windows Server 2016 Active Directory are present and enabled in the PRIV forest.  Launch a PowerShell window as administrator and type the following commands.

   ```
   $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
   Enable-ADOptionalFeature $of -scope ForestOrConfigurationSet -target "priv.contoso.local"
   ```
1. Acknowledge the change.

2. Type ADSIEdit.

3. Open the Actions menu, click “Connect To”. On the Connection point setting, change the naming context from “Default naming context” to “Configuration” and click OK.

4. After connecting, on the left side of the window below “ADSI Edit”, expand the Configuration node to see “CN=Configuration,DC=priv,....”. Expand CN=Configuration, and then expand CN=Services.

5. Right click on “CN=Shadow Principal Configuration” and click on Properties. When the properties dialog appears, change to the security tab.

6. Click Add. Specify the accounts “MIMService”, as well as any other MIM administrators who will later be performing New-PAMGroup to create additional PAM groups. For each user, in the allowed permissions list, add “Write”, “Create all child objects”, and “Delete all child objects”. Add the permissions.

7. Click Advanced to change to the Advanced Security settings. Select the line that allows MIMService access, and click Edit. Change the “Applies to” setting to “to this object and all descendant objects”. Update this permission setting and close the security dialog box.

8. Close ADSI Edit.

9. Next, authorize the MIM administrators to create and update authentication policy. Launch an elevated **Command prompt** and type the following commands, substituting the name of your MIM administrator account for “mimadmin” in each of the four lines:
    ```
    dsacls "CN=AuthN Policies,CN=AuthN Policy Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

    dsacls "CN=AuthN Policies,CN=AuthN Policy Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g mimadmin:CCDC;msDS-AuthNPolicy

    dsacls "CN=AuthN Silos,CN=AuthN Policy Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

    dsacls "CN=AuthN Silos,CN=AuthN Policy Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g mimadmin:CCDC;msDS-AuthNPolicySilo
    ```

10. Restart the PRIVDC server, and other domain controllers, so that these changes take effect.

## Step 5 - Update the MIM PAM configuration

1. Stop accepting new PAM requests, and wait for existing PAM requests to be completed.
1. Make sure you're signed in into the server hosting MIM Service as a MIM administrator.
1. Launch PowerShell.
1. Type the following command.

    ```powershell
    Set-PAMConfiguration -ForestFunctionality DS_BEHAVIOR_2015
    ```

1. Restart the server where MIM Service is hosted.

