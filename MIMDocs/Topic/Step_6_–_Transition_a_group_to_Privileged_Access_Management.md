---
title: Step 6 – Transition a group to Privileged Access Management
ms.custom: 
  - Identity Management
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
---
# Step 6 – Transition a group to Privileged Access Management
The privileged account creation in the PRIV forest is done using several new PowerShell cmdlets.  These cmdlets perform the following functions:

-   Creates a new group in the *PRIV* forest with the same *SID* (Security Identifier) as a group in the *CORP* forest and as an object in the MIM Service database corresponding to the group in the *PRIV* forest.

-   For each user account, the cmdlets creates two objects in the MIM Service database, corresponding to the user in the *CORP* forest and the new user account in the *PRIV* forest.

-   Creates a *PAM Role* object in the MIM Service database.

    In this prerelease preview, the cmdlets needs to be run once for each group, and once for each member of a group.  (Note that the migration cmdlets do not change or modify any user or groups in the *CORP* forest: that is to be done manually by the PAM administrator subsequently.)

    1.  Log into *PAMSRV* as a domain administrator and verify the services are running.

        1.  Ensure you are logged into *PAMSRV* as *PRIV\Administrator*.

        2.  Open **Services**.

        3.  Verify that the **Forefront Identity Manager** service is running.  If the service is not running, right-click on the service and select **start** to start the service.

    2.  Run the MIM PAM group and user management cmdlets to copy the CorpAdmins group and its member, Jen, from *CONTOSO* to *PRIV* domain.

        1.  Launch PowerShell and run the following commands, specifying the *CORP* domain admin (*CONTOSO\Administrator*) password where prompted:

            ```
            Import-Module MIMPAM
            Import-Module ActiveDirectory
            $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
            $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca 
            $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen 
            $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
            Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
            Set-ADUser –identity priv.Jen –Enabled 1 
            Add-ADGroupMember "Protected Users" priv.Jen
            $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
            ```
            For reference, the **New-PAMGroup** command takes the following parameters:

            -   The CORP forest domain name in NetBIOS form

            -   The name of the group to copy from that domain

            -   The CORP forest Domain Controller NetBIOS name

            -   The credentials of an domain admin user in the CORP forest

        Next, you will transition a user who is currently group member to JIT elevation, and then verify that cross-forest access rights are effective or the user's administrator account.

    3.  On *CORPDC*, remove Jen’s account from the *CONTOSO CorpAdmins* group, if it is still present.

        1.  Log into *CORPDC* as *CONTOSO\Administrator*.

        2.  Launch PowerShell, run the following command and confirm the change.

            ```
            Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
            ```

