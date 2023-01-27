---
title: "Configuring the Microsoft Identity Manager connector for Microsoft Graph for B2B| Microsoft Docs"
author: billmath

description: Microsoft Graph connector is external user AD account lifecycle management. In this scenario, an organization has invited guests into their Azure AD directory, and wishes to give those guests access to on-premises Windows-Integrated Authentication or Kerberos-based applications
keywords:
ms.author: billmath
manager: daveba
ms.date: 01/27/2023
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338

---

Azure AD business-to-business (B2B) collaboration with Microsoft Identity Manager(MIM) 2016 SP1 with Azure Application Proxy
============================================================================================================================

The initial scenario is external user AD account lifecycle
management.   In this scenario, an organization has invited guests into their Azure AD directory, and wishes to give those guests access to on-premises Windows-Integrated Authentication or Kerberos-based applications, via the [Azure AD application proxy](/azure/active-directory/app-proxy/application-proxy-add-on-premises-application) or other gateway mechanisms. The Azure AD application proxy requires each user to have their own AD DS account, for identification and delegation purposes.

## Scenario-Specific Guidance

A few assumptions made in the configuration of B2B with MIM and Azure AD
Application Proxy:

-   You have already deployed an on-premises AD, and Microsoft Identity Manager is installed and basic configuration of MIM Service, MIM Portal, Active Directory Management Agent (AD MA) and FIM Management Agent (FIM MA). For more information, see [Deploy Microsoft Identity Manager 2016 SP2](./microsoft-identity-manager-deploy.md).

-   You have already followed the instructions in the article on how to download and install the [Graph connector](microsoft-identity-manager-2016-connector-graph.md).

-   You have Azure AD Connect configured for synchronizing users and groups to Azure AD.

-   You have already set up Application Proxy connectors and connector groups. If not, see [Tutorial: Add an on-premises application for remote access through Application Proxy in Azure Active Directory](/azure/active-directory/app-proxy/application-proxy-add-on-premises-application#install-and-register-a-connector) to install and configure.

-   You have already published one or more applications, which rely on Windows Integrated Authentication or individual AD accounts via Azure AD App Proxy.

-   You have invited or you invite one or more guests, that have resulted in one or more users being created in Azure AD. For more information, see [Self-service for Azure AD B2B collaboration sign-up](/azure/active-directory/active-directory-b2b-self-service-portal).

## B2B End to End Deployment Example scenario

This guide builds on the following scenario:

Contoso Pharmaceuticals works with Trey Research Inc. as part of their R&D
Department. Trey Research employees need to access the research reporting
application provided by Contoso Pharmaceuticals.

-   Contoso Pharmaceuticals are in their own tenant, to have
    configured a custom domain.

-   Someone has invited an external user to the Contoso Pharmaceuticals tenant.
    This user has accepted the invitation and can access resources that are
    shared.

-   Contoso Pharmaceuticals has published an application via App Proxy. In this scenario, the example application is 
    the MIM Portal. This would enable a guest user to participate in MIM processes, for example in help desk scenarios or to request access to groups in MIM.


## Configure AD and Azure AD Connect to exclude users added from Azure AD

By default, Azure AD Connect will assume that non-admin users in Active Directory need to be synchronized into Azure AD.  If Azure AD Connect finds an existing user in Azure AD that matches the user from on-premises AD, Azure AD Connect will match the two accounts and assume that this is an earlier synchronization of the user, and make the on-premises AD authoritative.  However, this default behavior is not suitable for the B2B flow, where the user account originates in Azure AD. 

Therefore, the users brought into AD DS by MIM from Azure AD need to be stored in a way that Azure AD will not attempt to synchronize those users back to Azure AD.
One way to do this is to create a new organizational unit in AD DS, and configure Azure AD Connect to exclude that organizational unit.  

For more information, see [Azure AD Connect sync: Configure filtering](/azure/active-directory/hybrid/how-to-connect-sync-configure-filtering).

## Create the Azure AD application 

Note: Before creating in MIM Sync the management agent for the graph connector, make sure you have reviewed the guide to deploying the [Graph Connector](microsoft-identity-manager-2016-connector-graph.md), and created an application with a client ID and secret.
Ensure that the application has been authorized for least one of these permissions: `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` or `Directory.ReadWrite.All`. 

## Create the New Management Agent


In the Synchronization Service Manager UI, select **Connectors** and **Create**.
Select **Graph (Microsoft)** and give it a descriptive name.

![Screenshot showing Management agent for Graph with a name of B 2 B Graph, and an O K button.](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### Connectivity

On the Connectivity page, you must specify the Graph API Version. Production
ready PAI is **V 1.0**, Non-Production is **Beta**.

![Screenshot showing the Graph A P I version selected and a Next button.](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### Global Parameters

![Screenshot showing values for global parameters and a Next button.](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### Configure Provisioning Hierarchy

This page is used to map the DN component, for example OU, to the object type
that should be provisioned, for example organizationalUnit. This is not needed for this scenario, so leave this as the default and click next.

![Screenshot showing the Configure Provisioning Hierarchy page and a Next button.](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### Configure Partitions and Hierarchies

On the partitions and hierarchies page, select all namespaces with objects you
plan to import and export.

![Screenshot showing the Configure Partitions and Hierarchies page and an O K button.](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### Select Object Types

On the object types page, select the object types you
plan to import. You must select at least 'User'.

![Screenshot showing the Select Object Types page with an object type selected, and an O K button.](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### Select Attributes

On the Select Attributes screen, select attributes from Azure AD which will be needed to manage B2B users in AD. The Attribute "ID" is required.  The attributes `userPrincipalName` and `userType` will be used later in this configuration.  Other attributes are optional, including

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![Screenshot showing the Select Attributes screen with some attributes selected, and an O K button.](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### Configure Anchors

On the Configure Anchor screen, configuring the anchor attribute is a required step. By default, use the ID attribute for user mapping.

![Screenshot showing the Configure Anchors screen with an object type of user and an anchor attribute of i d, and a Next button.](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### Configure Connector Filter

On the configure Connector Filter page, MIM allows you to filter out objects based on attribute filter. In this scenario for B2B, the goal is to only  bring in Users with the value of the `userType` attribute that equals `Guest`, and not users with the userType that equals `member`.

![Screenshot showing the Configure Connector Filter page with filters for user selected, and an O K button.](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### Configure Join and Projection Rules

This guide assumes you will be creating a sync rule.  As configuring Join and Projection rules are handled by sync rule, it is not needed have to identify a join and projection on the connector itself. Leave default and click ok.

![Screenshot showing the Configure Join and Projection Rules page with an O K button.](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### Configure Attribute Flow

This guide assumes you will be creating a sync rule.  Projection is not needed to define the attribute flow in MIM Sync, as it is handled by the sync rule that is created later. Leave default and click ok.

![Screenshot showing the Configure Attribute Flow page with an O K button.](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### Configure Deprovision

The setting to configure deprovision allows you to configure MIM sync to delete the object, if the metaverse object is deleted. In this scenario, we make them disconnectors as the goal is to leave them in Azure AD. In this scenario, we are not exporting anything to Azure AD, and the connector is configured for Import only.

![Screenshot showing the Configure Deprovisioning page with an O K button.](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### Configure Extensions

Configure Extensions on this management agent is an option but not required because we are using a synchronization rule. If we decided to use an advanced rule in the attribute flow earlier, then there would be an option to define the rules extension.

![Screenshot showing the Configure Extensions page with an O K button.](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## Extending the metaverse schema


Before creating the sync rule, we need to create an attribute called userPrincipalName tied to the person object using the MV Designer.

In the Synchronization client, select Metaverse Designer

![Screenshot showing the Metaverse Designer option on the Synchronization Service Manager ribbon menu.](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Then Select the Person Object Type

![Screenshot showing the Metaverse Designer object types with the person object type selected.](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Next under actions click Add Attribute

![Screenshot showing the Add Attribute item on the Actions menu.](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Then complete the following details

Attribute name: **userPrincipalName**

Attribute Type: **String (Indexable)**

Indexed = **True**

![Screenshot showing dialogue boxes to enter values for Attribute name, Attribute type, and Indexed.](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## Creating MIM Service Synchronization Rules

In the steps below we begin the mapping of B2B guest account and the attribute flow. Some assumptions are made here: that you already have the Active Directory MA configured and the FIM MA configured to bring users to the MIM Service and Portal.

![Screenshot showing the Synchronization Rules screen.](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

The next steps will require the addition of  minimal configuration to the FIM MA and the AD MA.

More details can be found here for the configuration
<https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> - How Do I Provision Users to AD DS

### Synchronization Rule: Import Guest User to MV to Synchronization Service Metaverse from Azure Active Directory<br>

Navigate to the MIM Portal, select Synchronization Rules, and click new.  Create an inbound synchronization rule for the B2B flow via the graph connector.
![Screenshot showing the General tab on the Create Synchronization Rule screen with the synchronization rule name entered.](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![Screenshot showing the Scope tab with Metaverse Resource Type, External System, External System Resource Type, and Filters.](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

On the relationship criteria step, be sure to select "Create resource in FIM".
![Screenshot showing the Relationship tab and Relationship Criteria.](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![Screenshot showing the Inbound Attribute Flow tab on the Synchronization Rule IN screen.](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Configure the following inbound attribute flow rules.  Be sure to populate the `accountName`,  `userPrincipalName` and `uid` attributes as they will be used later in this scenario :

| **Initial Flow Only** | **Use as Existence Test** | **Flow (Source Value ⇒ FIM Attribute)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | `[displayName⇒displayName](javascript:void(0);)`                        |
|                       |                           | `[Left(id,20)⇒accountName](javascript:void(0);)`                        |
|                       |                           | `[id⇒uid](javascript:void(0);)`                                         |
|                       |                           | `[userType⇒employeeType](javascript:void(0);)`                          |
|                       |                           | `[givenName⇒givenName](javascript:void(0);)`                            |
|                       |                           | `[surname⇒sn](javascript:void(0);)`                                     |
|                       |                           | `[userPrincipalName⇒userPrincipalName](javascript:void(0);)`            |
|                       |                           | `[id⇒cn](javascript:void(0);)`                                          |
|                       |                           | `[mail⇒mail](javascript:void(0);)`                                      |
|                       |                           | `[mobilePhone⇒mobilePhone](javascript:void(0);)`                        |

### Synchronization Rule: Create Guest User account to Active Directory 

This synchronization rule creates the user in Active Directory.  Be sure the flow for `dn` must place the user in the organizational unit which was excluded from Azure AD Connect.  Also, update the flow for `unicodePwd` to meet your AD password policy - the user will not need to know the password.  Note the value of `262656` for `userAccountControl` encodes the flags `SMARTCARD_REQUIRED` and `NORMAL_ACCOUNT`.

![Screenshot showing the General tab of the Synchronization Rule OUT screen.](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![Screenshot showing the Scope tab with Metaverse Resource Type, External System, External System Resource Type, and Outbound System Scoping Filter.](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![Screenshot showing the Outbound Attribute Flow tab.](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Flow Rules:

| **Initial Flow Only** | **Use as Existence Test** | **Flow (FIM Value ⇒ Destination Attribute)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | `[accountName⇒sAMAccountName](javascript:void(0);)`                     |
|                       |                           | `[givenName⇒givenName](javascript:void(0);)`                            |
|                       |                           | `[mail⇒mail](javascript:void(0);)`                                      |
|                       |                           | `[sn⇒sn](javascript:void(0);)`                                          |
|                       |                           | `[userPrincipalName⇒userPrincipalName](javascript:void(0);)`            |
| **Y**                 |                           | `["CN="+uid+",OU=B2BGuest,DC=contoso,DC=com"⇒dn](javascript:void(0);)` |
| **Y**                 |                           | `[RandomNum(0,999)+userPrincipalName⇒unicodePwd](javascript:void(0);)`  |
| **Y**                 |                           | `[262656⇒userAccountControl](javascript:void(0);)`                      |

### Optional Synchronization Rule: Import B2B Guest User Objects SID to allow for login to MIM 

This inbound synchronization rule brings the user's SID attribute from Active Directory back into MIM, so the user can access the MIM Portal.  The MIM Portal requires that the user have the attributes `samAccountName`, `domain` and `objectSid` populated in the MIM Service database.

Configure the source external system as the `ADMA`, as the `objectSid` attribute will be set automatically by AD when MIM creates the user.
 
Note that if you configure users to be created in MIM Service, ensure that they are not in scope of any sets intended for employee SSPR management policy rules.  You may need to change your set definitions to exclude users who have been created by the B2B flow. 

![Screenshot showing the General tab of the Synchronization Rule IN screen.](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![Screenshot showing the Relationship tab of the Synchronization Rule IN screen.](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![Screenshot showing the Scope tab of the Synchronization Rule IN screen.](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![Screenshot showing the Inbound Attribute Flow tab.](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Initial Flow Only** | **Use as Existence Test** | **Flow (Source Value ⇒ FIM Attribute)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | `[sAMAccountName⇒accountName](javascript:void(0);)`                     |
|                       |                           | `["CONTOSO"⇒domain](javascript:void(0);)`                            |
|                       |                           | `[objectSid⇒objectSid](javascript:void(0);)`                                      |


## Run the synchronization rules

Next, we invite the user, and then run the management agent sync rules in the following
order:

-   Full Import and Synchronization on the `MIMMA` Management Agent.  This ensures MIM Sync has the latest synchronization rules configured.

-   Full Import and Synchronization on the `ADMA` Management Agent.  This ensures that MIM and Active Directory are consistent.  At this point, there will not yet be any pending exports for guests.

-   Full Import and Synchronization on the B2B Graph Management Agent.  This brings in the guest users into the metaverse.  At this point, one or more accounts will be pending export for `ADMA`.  If there are no pending exports, then check that guest users were imported into the connector space, and that the rules were configured for them to be given AD accounts.

-   Export, Delta Import, and Synchronization on the `ADMA` Management
    Agent.  If the exports failed, then check the rule configuration and determine if there were any missing schema requirements. 

-   Export, Delta Import, and Synchronization on the `MIMMA` Management Agent.  When this completes, there should no longer be any pending exports.

![Table listing Management Agents by Name, Type, Description, and State.](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## Optional: Application Proxy for B2B guests logging into MIM Portal

Now that we have created the synchronization rules in MIM. In the App Proxy configuration, define use the cloud principal to allow for KCD on app proxy.
Also, next added the user manually to the manage users and groups. The
options not to show the user until creation has occurred in MIM to add the guest
to an office group once provisioned requires a bit more configuration not
covered in this document.

![Screenshot showing the MIM B 2 B manage users and groups screen.](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![Screenshot showing the MIM B 2 B manage single sign-on screen.](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![Screenshot showing the MIM B 2 B manage application proxy screen.](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


Once all configured, have B2B user login and see the application.

![Screenshot showing demonstration login and apps.](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![Screenshot showing the Microsoft Identity Manager home page.](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

Next Steps
----------

[How Do I Provision Users to AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Functions Reference for FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[How to provide secure remote access to on-premises applications](/azure/active-directory/app-proxy/application-proxy)

[Download Microsoft Identity Manager connector for Microsoft Graph](https://go.microsoft.com/fwlink/?LinkId=717495)
