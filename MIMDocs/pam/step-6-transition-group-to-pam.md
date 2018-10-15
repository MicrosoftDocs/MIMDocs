---
# required metadata

title: Deploy PAM step 6 – Move group | Microsoft Docs
description: Migrate a group to the PRIV forest so that they can be managed with Privilege Access Management.
keywords:
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---
# Step 6 – Transition a group to Privileged Access Management

> [!div class="step-by-step"]
> [« Step 5 ](step-5-establish-trust-between-priv-corp-forests.md)
> [Step 7 »](step-7-elevate-user-access.md)

Privileged account creation in the PRIV forest is done using PowerShell cmdlets. These cmdlets perform the following functions:

- Create a new group in the PRIV forest with the same Security Identifier (SID) as a group in the CORP forest.  
- Create an object in the MIM Service database corresponding to the group in the PRIV forest.  
- For each user account, create two objects in the MIM Service database, corresponding to the user in the CORP forest and the new user account in the PRIV forest.  
- Create a PAM Role object in the MIM Service database.  

The cmdlets need to be run once for each group, and once for each member of a group. The migration cmdlets do not change or modify any user or groups in the CORP forest: the PAM administrator will do that manually afterwards.

1. Sign in to PAMSRV, either directly or from a PRIV workstation, as *PRIV\MIMAdmin*.

2.  Launch PowerShell, and type the following commands.

```PowerShell
   Import-Module MIMPAM
   Import-Module ActiveDirectory
```

3. Create a corresponding user account in PRIV for a user account in an existing forest, for demonstration purposes.

   Type the following commands into PowerShell.  If you did not use the name *Jen* to create the user in contoso.local earlier, then change the parameters of the command as appropriate. The password 'Pass@word1' is just an example and should be changed to a unique password value.

   ```PowerShell
       $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
       $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
       Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
       Set-ADUser –identity priv.Jen –Enabled 1
   ```

4. Copy a group and its member, Jen, from CONTOSO to PRIV domain, for demonstration purposes.

    Run the following commands, specifying the CORP domain admin (CONTOSO\Administrator) password where prompted:

   ```PowerShell
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
   ```

    For reference, the **New-PAMGroup** command takes the following parameters:

     -   The CORP forest domain name in NetBIOS form  
     -   The name of the group to copy from that domain  
     -   The CORP forest Domain Controller NetBIOS name  
     -   The credentials of an domain admin user in the CORP forest  

5. (Optional) On CORPDC, remove Jen’s account from the **CONTOSO CorpAdmins** group, if it is still present.  This is only needed for demonstration purposes, to illustrate how permissions can be associated with accounts created in the PRIV forest.

   1.  Sign in to CORPDC as *CONTOSO\Administrator*.

   2.  Launch PowerShell, run the following command and confirm the change.

       ```PowerShell
       Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
       ```


If you want to demonstrate that cross-forest access rights are effective for the user's administrator account, continue to the next step.

> [!div class="step-by-step"]
> [« Step 5 ](step-5-establish-trust-between-priv-corp-forests.md)
> [Step 7 »](step-7-elevate-user-access.md)
