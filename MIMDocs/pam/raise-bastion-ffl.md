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

As MIM 2016 went to general availability prior to Windows Server 2016, MIM PAM was designed to operate with either the Windows Server 2012 R2 or Windows Server 2016 functional level of the bastion forest.  For MIM to use Windows Sever 2012 R2, it implemented a "just in time" evaluation engine in the *MIM PAM component* service, to remove members from groups.  With Windows Server 2016, time-limited group memberships are built into Windows Server AD, as described in [What's new in Active Directory Domain Services for Windows Server 2016](/windows-server/identity/whats-new-active-directory-domain-services).  A existing deployment of MIM PAM should upgrade MIM and Windows Server of the bastion forest to ensure that it is using the built-in mechanism.

For an existing deployment, raising the functional level of the bastion forest requires additional configuration steps, both in Active Directory and MIM, so that time-limited memberships are enabled and MIM recognizes the new features of that functional level.

## Checking if you need to raise the functional level

1. Make sure you're signed in into the server hosting MIM Service as a MIM administrator.
2. Launch PowerShell.
3. Type the following command.

    ```powershell
    (Get-PAMConfiguration).ForestFunctionality
    ```

    If the output is `DS_BEHAVIOR_WIN2012R2`, then you may need to raise the functional level and complete the MIM and PAM configuration, as outlined in the rest of this article.  If the behavior is `DS_BEHAVIOR_WIN2015`, then you don't need to raise the functional level.

## Step 1 - Upgrade Windows Server and MIM software

Before raising the functional level, ensure that the domain controllers, member servers and MIM meet the minimum version requirements.

* All domain controllers in the bastion environment for the PRIV forest must be Windows Server 2016 or later.
* All member servers hosting MIM Service for PAM must be Windows Server 2016 or later.
* The MIM Service software must be MIM 2016 Service Pack 2 or a later hotfix.  See [Microsoft Identity Manager version history](../reference/version-history.md) for more information on the latest hotfixes.

## Step 2 - Raise the functional level

1. To perform this step, you must be a member of the Enterprise Admins group in the *PRIV* forest.

2. Open Active Directory Domains and Trusts.

3. In the console tree, right-click **Active Directory Domains and Trusts**, and then click **Raise Forest Functional Level**.

4. If the current functional level shown is **Windows Server 2016** or **Windows Server 2019**, then you do not need to change the functional level, but may need to make further AD or MIM changes.  Close Active Directory Domains and Trusts and continue at the next step.

5. In **Select an available forest functional level**, select the value **Windows Server 2016** , and then click **Raise**.

For more information on raising the functional level, or if an error occurs, see [how to raise Active Directory domain and forest functional levels](/troubleshoot/windows-server/identity/raise-active-directory-domain-forest-functional-levels).

## Step 3 - Update the domain controller configuration

## Step 4 - Update the MIM PAM configuration


