---
title: Planning a bastion environment
ms.custom: na
ms.prod: identity-manager-2015
ms.reviewer: na
ms.service: active-directory
ms.suite: na
ms.technology: 
  - active-directory-domain-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
author: Kgremban
---
# Planning a bastion environment
 
> [!Note] {This content is derived from the white paper "Mitigating Pass-the-Hash and Other Credential Theft, version 2", developed by Microsoft Corporation Trustworthy Computing group.}

## Overview
Adding a bastion environment with a dedicated administrative forest to an Active Directory enables organizations to easily manage administrative accounts, workstations, and groups in an environment that has stronger security controls than their existing production environment.

This architecture enables a number of controls that aren’t possible or easily configured in a single forest architecture. This approach allows the provisioning of accounts as standard non-privileged users in the administrative forest that are highly privileged in the production environment, enabling greater technical enforcement of governance. This architecture also enables the use of the selective authentication feature of a trust as a means to restrict logons (and credential exposure) to only authorized hosts. In situations in which a greater level of assurance is desired for the production forest without incurring the cost and complexity of a complete rebuild, an administrative forest can provide an environment that increases the assurance level of the production environment.

Additional techniques can be used in addition to the dedicated administrative forest. These include restricting where administrative credentials are exposed, limiting role privileges of users in that forest, and ensuring administrative tasks are not performed on hosts used for standard user activities (for example, email and web browsing).

## Bastion environment forest considerations

A dedicated administrative forest is a standard single domain Active Directory forest dedicated to the function of Active Directory management. Administrative forests and domains may be hardened more stringently than production forests because of the limited use cases. Furthermore, as this forest is separated and does not trust the organization's existing forests, should one the organization's existing forest is later found to have been compromised, the compromise would not extend to this dedicated forest.

An administrative forest design has the following considerations:

**Limited scope** – The value of an admin forest is the high level of security assurance and reduced attack surface resulting in lower residual risk. The forest can be used to house additional management functions and applications, but each increase in scope will increase the attack surface of the forest and its resources. The objective is to limit the functions of the forest and admin users inside to keep the attack surface minimal, so each scope increase should be considered carefully. The accounts in a dedicated administrative forest should be in a single tier, typically either tier 0 or tier 1, and if tier 1 may further be limited to a particular scope of application (e.g., finance apps) or user community (e.g., outsourced IT vendors).

**Restricted trust** – Configure trust from managed forests(s) or domain(s) to the administrative forest.

- A one-way trust is required from production environment to the admin forest. This can be a domain trust or a forest trust. The admin forest domain does not need to trust the managed domains and forests to manage Active Directory, though additional applications may require a two-way trust relationship, security validation, and testing.

- Selective authentication should be used to restrict accounts in the admin forest to only logging on to the appropriate production hosts. For maintaining domain controllers and delegating rights in Active Directory, this typically requires granting the “Allowed to logon” right for domain controllers to designated Tier 0 admin accounts in the admin forest. See [Configuring Selective Authentication Settings](http://technet.microsoft.com/library/cc755844%28v=ws.10%29.aspx) for more information.

## Deploying the forest

At its core, the bastion environment will include a collection of Active Directory domain controller servers for authentication and authorization, and Microsoft Identity Manager servers for privileged account lifecycle management and request workflow processing. It will also include privileged administrator workstations for administrators to authenticate.

### Maintaining logical separation

In order to ensure that the bastion environment is not impacted by existing or future security incidents in the existing organizational Active Directory, the following guidelines should be used when preparing systems for the bastion environment:

- Windows Servers should not be domain joined or leverage software or settings distribution from the existing environment.

- The bastion environment must contain its own Active Directory Domain Services, providing Kerberos and LDAP, DNS, time and time services, to the bastion environment. The servers providing Active Directory Domain Services must be at Windows Server 2012 R2 or later, and the forest functional level must be Windows Server 2012 R2 level or later.

- Microsoft Identity Manager requires SQL Server 2012 Service Pack 1 or SQL Server 2014. This should be deployed on dedicated servers in the bastion environment; MIM should not use a SQL database farm in the existing environment.

- The bastion environment requires Microsoft Identity Manager 2016 deployed for the PAM scenario, specifically the MIM Service and PAM components must be deployed.

- Backup software and media for the bastion environment must be kept separate from that of systems in the existing forests, so that an administrator in the existing forest cannot subvert a backup of the bastion environment.

- Users who manage the bastion environment servers must log in from workstations that are not accessible to administrators in the existing environment, so that the credentials for the bastion environment are not leaked. Deploying and hardening dedicated administrative workstations is described further below.

### Ensuring availability of administration services from the bastion environment

As administration of applications in the existing forest will be transitioned to the bastion environment, planning the deployment of the bastion environment needs to take into account how to provide sufficient availability to meet the requirements of those applications. The techniques include:

- Deploy Active Directory Domain Services on multiple computers in the bastion environment. At least two are necessary to ensure continued authentication, even if one server is temporarily restarted for scheduled maintenance. Additional computers may be necessary for higher load or if the organization has resources and administrators based in multiple geographic regions.

- Prepare break glass accounts in the existing forest, and in the dedicated admin forest, for emergency purposes.

- Deploy SQL Server and MIM Service on multiple computers in the bastion environment.

- Maintain a backup copy, as of each change to users or role definitions in the dedicated admin forest, of AD and SQL for disaster recovery.

### Configuring appropriate Active Directory permissions

The administrative forest should be configured to least privilege based on the requirements for Active Directory administration.

- Accounts in the admin forest that are used to administer the production environment should not be granted administrative privileges to the admin forest, domains in it, or workstations in it.

For example, if Alice is responsible for operations of the bastion environment, Bob is a role owner for Exchange administration, and Carol is an Exchange administrator, then in the dedicated administrative forest, Neither Bob nor Carol would need to be in the Administrators group in the dedicated administrative environment.

- Administrative privileges over the admin forest itself should be tightly controlled by an offline process to reduce the opportunity for an attacker or malicious insider to erase audit logs. This also helps ensure that personnel with production admin accounts cannot relax the restrictions on their accounts and increase risk to the organization.

- The administrative forest should follow the Microsoft Security Compliance Manager (SCM) configurations for the domain, including strong configurations for authentication protocols.

When creating the bastion environment, before installing Microsoft Identity Manager, identify and create the accounts that will be used for administration within this environment. This will include:

- **Break glass accounts** These accounts should only be able to log into the domain controllers in the bastion environment.

- **"Red Card" administrators** These user accounts are intended for provisioning of other accounts, and performing unscheduled maintenance. No access to existing forests or systems outside of the bastion environment is provided to these accounts. The credentials, e.g., a smartcard, should be physically secured, and use of these account should be logged.

- **Service accounts** needed by Microsoft Identity Manager, SQL Server, and other software installed in the bastion environment.

### Integrating with SIEM and behavior analysis tools

Detective controls for the administrative forest should be designed to alert on anomalies in the admin forest. The limited number of authorized scenarios and activities can help tune these controls more accurately than the production environment. This can include exporting the logs from Active Directory and Microsoft Identity Manager to a SIEM system for analysis.

### Hardening the hosts

All hosts, including domain controllers, servers, and workstations joined to the administrative forest, should have the latest operating systems, service packs installed and kept up to date. Furthermore,

- The applications required for performing administration should be pre-installed on workstations so that accounts using them don’t need to be in the local administrators group to install them. Domain Controller maintenance can typically be performed with RDP and Remote Server Administration Tools.

- Admin forest hosts should be automatically updated with security updates. While this may create risk of interrupting domain controller maintenance operations, it provides a significant mitigation of security risk of unpatched vulnerabilities.

Windows Server Update Services can be configured to automatically approve updates. For more information, see the “Automatically Approve Updates for Installation” section in Approving Updates.

#### Workstation hardening

Any hosts on which administrators enter credentials or perform administrative tasks are entrusted with the privileges associated with the account that is used, even if temporarily. The act of physically typing a password, smartcard PIN, or other verifier, or connecting a physical authentication device grants the credentials’ permissions to that computer. The risk of a system should be measured by the highest risk activity that is performed on it, such as Internet browsing, sending and receiving email, or the use of other applications that process unknown or untrusted content.

Administrative hosts include the following computers:

- A desktop on which credentials of the administrator are physically typed or entered.

- Administrative “jump servers” on which administrative sessions and tools are run.

- All hosts on which administrative actions are performed, including those that use a standard user desktop running an RDP client to remotely administer servers and applications.

- Servers that host applications that need to be administered, and are not accessed using RDP with Restricted Admin Mode or Windows PowerShell remoting.

#### Deploying dedicated administrative workstations

Although inconvenient, separate hardened workstations dedicated to users with high-impact administrative credentials may be required to provide a host with a level of security that is equal to or greater than the level of the privileges entrusted to the credentials. Maintaining security against a determined and talented adversary may require additional measures, such as:

- **Verification of all media in build as clean** to mitigate against malware installed in a master image or injected into an installation file during download or storage.

- **Security Baselines** should be used as starting configurations.

Customers can use the Microsoft Security Compliance Manager (SCM) for configuring the baselines on the administrative hosts.

- **Secure Boot** to mitigate against attackers or malware attempting to load unsigned code into the boot process.

This feature was introduced in Windows 8 to leverage the Unified Extensible Firmware Interface (UEFI).

- **Software restriction** to ensure that only authorized administrative software is executed on the administrative hosts.

Customers can use AppLocker for this task with a whitelist of authorized applications, to help prevent malicious software and unsupported applications from executing.

- **Full volume encryption** to mitigate against physical loss of computers, such as administrative laptops used remotely.

- **USB restrictions** to protect against physical infection vectors.

- **Network isolation** to protect against network attacks and inadvertent admin actions. Host firewalls should block all incoming connections except those explicitly required and block all unneeded outbound Internet access.

- **Antimalware** to protect against known threats and malware.

- **Exploit mitigations** to mitigate against unknown threats and exploits, including the Enhanced Mitigation Experience Toolkit (EMET).

- **Attack surface analysis** to prevent introduction of new attack vectors to Windows during installation of new software.

Use of tools such as the Attack Surface Analyzer (ASA) will help assess configuration settings on a host and identify attack vectors introduced by software or configuration changes.

- Users should not need local computer administrative privileges.

- Outgoing RDP sessions should have RestrictedAdmin mode, except when required by the role. See [What's New in Remote Desktop Services in Windows Server](https://technet.microsoft.com/en-us/library/dn283323.aspx) for more information.

Some of these measures might seem extreme, but public revelations in recent years have illustrated the significant capabilities that skilled adversaries possess to compromise targets.

### Establishing trust and preparing existing domains for management from the bastion environment

MIM includes PowerShell cmdlets to assist in establishing trust between the existing AD domains and the dedicated administrative forest in the bastion environment. After the bastion environment is deployed, and before any users or groups are converted to JIT, then the New-PAMTrust and New-PAMDomainConfiguration cmdlets will update the domain trust relationships and create artifacts needed for AD and MIM.

When the existing Active Directory topology changes, the `Test-PAMTrust`, `Test-PAMDomainConfiguration`, `Remove-PAMTrust` and `Remove-PAMDomainConfiguration` cmdlets can be used to update the trust relationships.

#### Procedures for establishing trust for each forest

The New-PAMTrust cmdlet must be run once for each existing forest. It is invoked on the MIM Service computer in the administrative domain. The parameters to this command are the domain name of the top domain of the existing forest, and credential of an administrator of that domain.

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

After establishing the trust, then configure each domain to enable management from the bastion environment, as described in the next section.

#### Procedures for enabling management of each domain

There are seven requirements for enabling management for an existing domain.

1. There must be a group in the existing domain, whose name is the NetBIOS domain name followed by three dollar signs, e.g., "CONTOSO$$$". The group scope must be "domain local" and the group type must be "Security". This will be needed for groups to be created in the dedicated administrative forest with the same Security identifier as groups in this domain. This can be done with the following PowerShell command, performed by an administrator of the existing domain and run on an workstation joined to the existing domain:

```
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

2. The group policy settings on the domain controller for auditing, must include both success and failure auditing for Audit account management and Audit directory service access. This can be done with the Group Policy management console, performed by an administrator of the existing domain and run on a workstation joined to the existing domain:

3. **Configure auditing** Go to Start, Administrative Tools, and then launch Group Policy Management.

4. Navigate to Forest: contoso.local, Domains, contoso.local, Domain Controllers, Default Domain Controllers Policy. An informational message will appear.

![pam-group-policy-management-editor](/Image/pam-group-policy-management.jpg)

5. Right-click on Default Domain Controllers Policy and select Edit... in the Right-click menu. A new window will appear.

6. In the Group Policy Management Editor window, under the Default Domain Controllers Policy tree, navigate to and expand Computer Configuration, Policies, Windows Settings, Security Settings, Local Policies, Audit Policy.

![pam-group-policy-management-editor](/Image/pam-group-policy-management-editor.jpg)

5. In the details pane, right click on Audit account management and select Properties in the right-click menu. Click Define these policy settings, put a checkbox on Success, put a checkbox on Failure, click Apply and OK.

6. In the details pane, right click on Audit directory service access and select Properties in the right-click menu. Click Define these policy settings, put a checkbox on Success, put a checkbox on Failure, click Apply and OK.

![pam-group-policy-management-editor2](/Image/pam-group-policy-management-editor2.jpg)


7. Close the Group Policy Management Editor window, the Group Policy Management window. Then apply the audit settings by launching a PowerShell window and typing:

```
gpupdate /force /target:computere
```
   
The message “Computer Policy update has completed successfully.” should appear after a few minutes.

8. The domain controllers must allow RPC over TCP/IP connections for LSA from the bastion environment. On older versions of Windows Server, TCP/IP support in LSA must be enabled in the registry:

```
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

9. The `New-PAMDomainConfiguration` cmdlet must be run on the MIM Service computer in the administrative domain. The parameters to this command are the domain name of the existing domain, and credential of an administrator of that domain.

```
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

10. The accounts in the bastion forest used to establish roles (admins who use the `New-PAMUser` and `New-PAMGroup` cmdlets), as well as the account used by the MIM monitor service, need read permissions in that domain.

For example, if CORPDC is a domain controller of the existing domain CONTOSO, and PRIV\\Administrator is the user in the bastion environment who needs read access, then following steps enable read access to the domain by an administrator and the monitoring service.

11. **On CORPDC, enable read access to AD by PRIV administrators and the monitoring service.** Ensure you are logged into CORPDC as a Contoso domain administrator (such as Contoso\Administrator).

12. Launch Active Directory Users and Computers.

13. Right click on the domain contoso.local and select Delegate Control.

14. On the Selected users and groups tab, click Add.

15. On the Select Users, Computers, or Groups popup, click Locations and change the location to priv.contoso.local. On the object name, type Domain Admins and click Check Names. When a popup appears, for the username type priv\administrator and the password.

16. After Domain Admins, type "; MIMMonitor". After the names Domain Admins and MIMMonitor are underlined, click OK, then click Next.

17. In the list of common tasks, select "Read all user information", then click Next and click Finish.

18. Close Active Directory Users and Computers.

> [!Note] {If the goal of the privileged access management project is to reduce the number of accounts with Domain Administrator privileges permanently assigned to the domain, there must be a *break glass* account in the domain, in case there is a later problem with the trust relationship. Accounts for emergency access to the production forest should exist in each domain, and should only be able to log into domain controllers. For organizations with multiple sites, additional accounts may be required for redundancy.}

19. Finally, review the permissions on the *AdminSDHolder* object in the System container in that domain. The *AdminSDHolder* object has a unique Access Control List (ACL), which is used to control the permissions of security principals that are members of built-in privileged Active Directory groups. Note if any changes to the default permissions have been made that would impact users with administrative privileges in the domain, since those permissions will not apply to users whose account is in the bastion environment.

### Selecting users and groups for inclusion

The next step is defining the PAM roles, associating the users and groups to which they should have access. This will typically be a subset of the users and groups for the tier identified as being managed in the bastion environment. More information is in [*Defining roles for Privileged Access Management*](https://technet.microsoft.com/library/mt620091.aspx).
