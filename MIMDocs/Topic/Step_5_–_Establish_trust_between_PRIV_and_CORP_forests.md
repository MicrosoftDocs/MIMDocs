---
title: Step 5 – Establish trust between PRIV and CORP forests
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
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
---
# Step 5 – Establish trust between PRIV and CORP forests
The *PRIV* and *CONTOSO* domain controllers are bound by a trust which allows for users in the *PRIV* domain to access resources on the CORP domain.

1.  Prior to establishing trust, each domain controller must be configured for DNS name resolution for its counterpart, based on the other domain controller/DNS server’s IP address.

    1.  Ensure that there are no other DNS servers which are providing domain naming services to computers in either domain.  If the virtual machines have network interfaces connected to public networks, it may be necessary to override the Windows network interface settings to ensure a DHCP-supplied DNS server address is not used by any virtual machines.

    2.  (Optional) On *CORPDC*, launch PowerShell, and type the following command.

        ```
        nslookup -qt=ns priv.contoso.local.
        ```
        Ensure that the output indicates a nameserver record for this domain.

    3.  (Optional) Alternatively, use **DNS Manager** (located in Start, Application Tools, DNS) to confirm DNS name forwarding for the *PRIV* domain to *PRIVDC’s* IP address.  Using this program, expand the nodes *CORPDC, Forward Lookup Zones, contoso.local*, and ensure a key named *priv* is present as a Name Server (NS) type.

        ![](../Image/PAM_GS_DNS_Manager.png)

2.  On *PAMSRV*, establish one-way trust with *CORPDC* so that the CORP domain controller trusts the *PRIV* forest.

    1.  Ensure you are logged into *PAMSRV* as a *PRIV* domain administrator (such as *PRIV\Administrator*).

    2.  Launch PowerShell.

    3.  Type the following PowerShell commands, and enter the credential for the CORP domain administrator (e.g., *CONTOSO\Administrator*) when prompted, if needed.

        ```
        $ca = get-credential
        New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
        New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
        ```

3.  On *CORPDC*, enable read access to AD by PRIV administrators and the monitoring service.

    1.  Ensure you are logged into *CORPDC* as a Contoso domain administrator (such as *Contoso\Administrator*).

    2.  Launch **Active Directory Users and Computers**.

    3.  Right click on the domain **contoso.local** and select **Delegate Control**.

    4.  On the Selected users and groups tab, click **Add**.

    5.  On the **Select Users, Computers**, or **Groups** popup, click **Locations** and change the location to *priv.contoso.local*.  On the object name, type *Domain Admins* and click **Check Names.** When a popup appears, for the username type *priv\administrator* and the password.

    6.  After *Domain Admins*, type "*; MIMMonitor*". After the names Domain Admins and MIMMonitor are underlined, click **OK**, then click **Next**.

    7.  In the list of common tasks, select "**Read all user information**", then click **Next** and click **Finish**.

    8.  Close **Active Directory Users and Computer**s.

4.  On *PAMSRV*, start the **monitoring and component services**.

    1.  Ensure you are logged into *PAMSRV* as a *PRIV* domain administrator (such as *PRIV\Administrator*).

    2.  Launch PowerShell.

    3.  Type the following PowerShell commands.

        ```
        net start "PAM Component service"
        net start "PAM Monitoring service"
        ```

5.  (optional) Verify that SID history is enabled and SID filtering is disabled on the trust from the *CORP* domain to the *PRIV* domain.

    1.  Ensure you are logged into *CORPDC* as a domain administrator (such as *CONTOSO\Administrator*).

    2.  Open a PowerShell window.

    3.  Use **netdom** to ensure SID history is enabled and SID filtering is disabled.  Type:

        ```
        netdom trust contoso.local /quarantine /enablesidhistory:yes /domain priv.contoso.local
        ```
        The output should indicate either “**Enabling SID history for this trust**” or “**SID history is already enabled for this trust**”.

        The output should also indicate that “**SID filtering is not enabled for this trust**”. See [http://technet.microsoft.com/library/cc772816(v=WS.10).aspx](http://technet.microsoft.com/library/cc772816%28v=WS.10%29.aspx)  for more information.

