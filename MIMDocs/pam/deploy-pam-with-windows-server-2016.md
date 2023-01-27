---
# required metadata

title: Deploy MIM Privileged Access Management with Windows Server 2016 | Microsoft Docs
description: Learn about deploying Privileged Access Management with server 2016
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/27/2023
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid:

---



# Deploy MIM PAM with Windows Server 2016


This scenario enables MIM 2016 SP2 for the PAM scenario using features of Windows Server 2016 or later as the domain controller for the “PRIV” forest.  When this scenario is configured, a user’s Kerberos ticket will be time-limited to the remaining time of their role activations.

## Preparation

A minimum of two VMs are required for the lab environment:

-   VM hosts the PRIV Domain Controller, running Windows Server 2016 or later

-   VM hosts the MIM Service, running Windows Server 2016 or later

> [!NOTE]
> If you do not already have a “CORP” domain in your lab environment, an additional domain controller for that domain is required. The “CORP” domain controller can run either Windows Server 2016 or Windows Server 2012 R2.


Perform the install as described in the [Getting started guide](privileged-identity-management-for-active-directory-domain-services.md), **except as indicated below**:

- If you are creating a new CORP domain, when following the instructions in [Step 1 - Prepare the CORP domain controller](step-1-prepare-corp-domain.md), you can choose to optionally configure the CORP domain to be at the Windows Server 2016 functional level. **If you choose this option, make the following adjustments**:

  - If you are using Windows Server 2016 media, the installation option will be called Windows Server 2016 (Server with Desktop Experience).

  - You can Specify the Windows Server 2016 functional level for the CORP forest and domain by supplying 7 as the domain and forest version number in the argument to the Install-ADDSForest command, as follows:
    ```
    Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName contoso.local –DomainNetbiosName contoso –Force –NoDnsOnNetwork
    ```
  - In "Create new users and groups", the final command (New-ADGroup -name 'CONTOSO\$\$\$' …) is **not required when both CORP and PRIV domain controllers are Windows Server 2016 domain functional level**.

  - The changes described in "Configure auditing"(item #8) and "Configure registry settings" (item #10) are **recommended but not required** when both CORP and PRIV domain controllers are Windows Server 2016 domain functional level.

- If you choose to use Windows Server 2012 R2 as the operating system for CORPDC, you must install hotfixes 2919442, 2919355, [and update 3155495](https://support.microsoft.com/kb/3156418) on CORPDC.

- Follow the instructions in [Step 2 - Prepare PRIV domain controller](step-2-prepare-priv-domain-controller.md), and ensure if you were using a previous version of the content, to make these adjustments:

  -   Install using Windows Server 2016 media. The installation option will be called Windows Server 2016 (Server with Desktop Experience).

  -   In the "Add roles" instructions (item #4), **you must specify the domain and forest version numbers in the fourth line of the PowerShell commands to be 7**, to permit the Windows Server AD features described later to be enabled.

      ```
      Install-ADDSForest -DomainMode 7 -ForestMode 7 -DomainName priv.contoso.local  -DomainNetbiosName priv -Force -CreateDNSDelegation -DNSDelegationCredential $ca
      ```  

  -   When configuring the auditing and logon rights, note that the Group Policy Management program will be located in the Windows Administrative Tools folder.

  -   Configuring the registry settings needed for SID history migration (item #8) is **no longer required when the PRIV domain is Windows Server 2016 domain functional level**.

  -   After configuring delegation, and before restarting the server, enable the Privileged Access Management features in Windows Server 2016 Active Directory by launching a PowerShell window as administrator and typing the following commands.

  ```
  $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
  Enable-ADOptionalFeature $of -scope ForestOrConfigurationSet -target "priv.contoso.local"
  ```

  - After configuring delegation, and before restarting the server, authorize the MIM administrators and MIM Service account to create and update shadow principals.

    a. Launch a PowerShell window and type ADSIEdit.

    b. Open the Actions menu, click “Connect To”. On the Connection point setting, change the naming context from “Default naming context” to “Configuration” and click OK.

    c. After connecting, on the left side of the window below “ADSI Edit”, expand the Configuration node to see “CN=Configuration,DC=priv,....”. Expand CN=Configuration, and then expand CN=Services.

    d. Right click on “CN=Shadow Principal Configuration” and click on Properties. When the properties dialog appears, change to the security tab.

    e. Click Add. Specify the accounts “MIMService”, as well as any other MIM administrators who will later be performing New-PAMGroup to create additional PAM groups. For each user, in the allowed permissions list, add “Write”, “Create all child objects”, and “Delete all child objects”. Add the permissions.

    f. Change to Advanced Security settings. On the line which allows MIMService access, click Edit. Change the “Applies to” setting to “to this object and all descendant objects”. Update this permission setting and close the security dialog box.

    g. Close ADSI Edit.

  - After configuring delegation, and before restarting the server, authorize the MIM administrators to create and update authentication policy.

    a.  Launch an elevated **Command prompt** and type the following commands, substituting the name of your MIM administrator account for “mimadmin” in each of the four lines:
    ```
    dsacls "CN=AuthN Policies,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

    dsacls "CN=AuthN Policies,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:CCDC;msDS-AuthNPolicy

    dsacls "CN=AuthN Silos,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

    dsacls "CN=AuthN Silos,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:CCDC;msDS-AuthNPolicySilo
    ```


- Follow the instructions in [Step 3 - Prepare a PAM server](step-3-prepare-pam-server.md).

  -   Note that when installing on Windows Server 2016, the “ApplicationServer” role is no longer available.

  -   If installing MIM on Windows Server 2016, it is not possible to install SharePoint 2013.  If you wish to use the MIM Portal, you must use a later version of SharePoint.

- Follow the instructions in [Step 4 – Install MIM components on PAM server and workstation](step-4-install-mim-components-on-pam-server.md), with these adjustments.

  -   The user installing the MIM Service and PAM components must have write access to the PRIV domain in AD, as the MIM installation creates a new AD OU “PAM objects”.

  -   If SharePoint is not installed, do not install the MIM Portal.

- Follow the instructions in [Step 5 - Establish trust](step-5-establish-trust-between-priv-corp-forests.md) with these adjustments:

  - When establishing one-way trust, only perform the first two PowerShell commands (get-credential and New-PAMTrust), You do not perform the New-PAMDomainConfiguration command. Instead, after establishing trust, log onto PRIVDC as PRIV\\Administrator, launch PowerShell and type the following commands:
    ```
    netdom trust contoso.local /domain:priv.contoso.local /enablesidhistory:yes
    /usero:contoso\administrator /passwordo:Pass@word1

    netdom trust contoso.local /domain:priv.contoso.local /quarantine:no
    /usero:contoso\administrator /passwordo:Pass@word1  

    netdom trust contoso.local /domain:priv.contoso.local /enablepimtrust:yes
    /usero:contoso\administrator /passwordo:Pass@word1
    ```

- Item #5 (verification of trust) is no longer required when both CORP and PRIV domains are at Windows Server 2016 domain functional level.

## More information

- [Privileged Access Management for Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md)
- [Configure the MIM environment for Privileged Access Management](configuring-mim-environment-for-pam.md)
- [Configure PAM using scripts](sp1-pam-configure-using-scripts.md)
