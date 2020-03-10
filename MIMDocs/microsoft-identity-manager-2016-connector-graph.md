---
title: "The Microsoft Identity Manager connector for Microsoft Graph | Microsoft Docs"
author: billmath
description: Microsoft Identity Manager connector for Microsoft Graph enables external user AD account lifecycle management. In this scenario, an organization has invited guests into their Azure AD directory, and wishes to give those guests access to on-premises Windows-Integrated Authentication or Kerberos-based applications
keywords:
author: fimguy
ms.author: billmath
manager: daveba
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager

ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
---
# Microsoft Identity Manager connector for Microsoft Graph


## Summary 


The [Microsoft Identity Manager connector for Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)
 enables additional integration scenarios for Azure AD Premium customers.  It surfaces in the MIM sync metaverse additional objects obtained from the [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 and beta.

## Scenarios covered


### B2B account lifecycle management


The initial scenario for the Microsoft Identity Manager connector for Microsoft Graph is as a connector to help automate AD DS account lifecycle
management for external users. In this scenario, an organization is synchronizing employees to Azure AD from AD DS using Azure AD Connect, and has also invited guests into their Azure AD directory. Inviting a guest results in an external user object being in that organization's Azure AD directory, which is not in that organization's AD DS. Then the organization wishes to give those guests access to on-premises Windows
Integrated Authentication or Kerberos-based applications, via the [Azure AD application proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)
or other gateway mechanisms. The Azure AD application proxy requires each user to have their own AD DS account, for identification and delegation purposes.  

To learn how to configure MIM sync to automatically create and maintain AD DS accounts for guests, after reading the instructions in this article, continue reading in the article [Azure AD business-to-business (B2B) collaboration with MIM 2016 SP1 with Azure Application Proxy](~/microsoft-identity-manager-2016-graph-b2b-scenario.md).  That article illustrates the sync rules needed for the connector.

### Other identity management scenarios


The connector can be used for other specific identity management scenarios involving create, read, update and delete of user, group and contact objects in Azure AD, beyond user and group synchronization to Azure AD. When evaluating potential scenarios, please keep in mind: this connector cannot be operated in a scenario, which would result in a data flow overlap, actual or potential synchronization conflict with an Azure AD Connect deployment.  [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) is the recommended approach to integrate on-premises directories with Azure AD, by synchronizing users and groups from on-premises directories to Azure AD.  Azure AD Connect has many more synchronization features and enables scenarios such as password and device writeback, which are not possible for objects created by MIM. If data is being brought into AD DS, for example, ensure that it is excluded from Azure AD Connect attempting to match those objects back to the Azure AD directory.  Nor can this connector be used to make changes to Azure AD objects, which were created by Azure AD Connect.



## Preparing to use the Connector for Microsoft Graph

### Authorizing the connector to retrieve or manage objects in your Azure AD directory


1.  The connector requires a Web app / API application to be created in Azure AD, so that it can be authorized with appropriate permissions to operate on Azure AD objects through Microsoft Graph.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Picture 1. New application registration

2.  In the Azure portal, open the created application, and save the Application ID, as a Client ID to use later on the MA’s connectivity page:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Picture 2. Application ID

3.  Generate new Client Secret by opening All settings -\> Keys. Set some Key description and select needful Duration. Save changes. A secret
    value will not be available after leaving the page.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Picture 3. New Client Secret

4.  Add “Microsoft Graph API” to the application by opening “Required permissions.”

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Picture 4. Add new API

The following permission should be added to the application to allow it to use the “Microsoft Graph API”, depending on the scenario:

| Operation with object | Permission required                                                                  | Permission type |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Import Group          | `Group.Read.All` or `Group.ReadWrite.All`                                                | Application     |
| Import User           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` or `Directory.ReadWrite.All` | Application     |

More details about required permissions could be found [here](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

5. Grant the application the required permissions.


## Installing the connector


6.  Before you install the Connector, make sure you have the following on the synchronization server: 

 - Microsoft .NET 4.5.2 Framework or later
 - Microsoft Identity Manager 2016 SP1, and must use hotfix 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) or later.

7. The connector for Microsoft Graph, in addition to other connectors for Microsoft Identity Manager 2016 SP1, is available as a download from the
[Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  Restart MIM Synchronization Service.
 
## Connector configuration



9.  In the Synchronization Service Manager UI, select **Connectors** and **Create**.
Select **Graph (Microsoft)** , create a connector and give it a descriptive name.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. In the MIM synchronization service UI, specify  the Application ID and generated Client Secret. Each management agent configured in MIM Sync should have its own application in Azure AD to avoid running import in parallel for the same application.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Picture 5. Connectivity page

The connectivity page (Picture 5) contains the Graph API version that is used
and tenant name. The Client ID and Client Secret represent the Application ID and
Key value of the WebAPI application that must be created in Azure AD.

11. Make any necessary changes on the Global Parameters page:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Picture 6. Global Parameters page

Global parameters page contains the following settings:

- DateTime format – format that is used for any attribute with Edm.DateTimeOffset type. All dates are converted to string by using that format during the import. Set format is applied for any attribute, which
saves date.

 - HTTP timeout (seconds) – timeout in seconds that will be used during each HTTP call to WebAPI application.

 - Force change password for created user at next sign – this option is used for new user that will be created during the export. If option is enabled, then [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) property will be set to true, otherwise it will be false.

## Configuring the connector schema and operations


12.   Configure the schema.  The connector supports the following list of object types:

-   User

    -   Full/Delta Import

    -   Export (Add, Update, Delete)

-   Group

    -   Full/Delta Import

    -   Export (Add, Update, Delete)


The list of attribute types that are supported:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (string in connector space)

-   `microsoft.graph.directoryObject` (reference in connector space to any of the
    supported objects)

-   `microsoft.graph.contact`

Multivalued attributes (Collection) are also supported for any of a type from the list above.

The connector uses the ‘`id`’ attribute for anchor and DN for all objects.  Therefore, rename is not needed, because Graph API does not allow an object to change its ‘id’ attribute.


## Access token lifetime


A Graph application requires an access token for accessing the Graph API. A connector
will request a new access token for each import iteration (import iteration depends on
page size). For example:

-   Azure AD contains 10000 objects

-   Page size configured in connector is 5000

In this case there will be two iterations during the import, each of them will return 5000 objects to Sync. So, a new access token will be request twice.

During the export a new access token will be requested for each object that must be added/updated/deleted.

## Troubleshooting


**Enable logs**

If there are any issues in Graph, then logs could be used to localize the problem. So, traces could be enabled in [the same way like for Generic connectors](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Or just by adding the following to `miiserver.exe.config` (inside `system.diagnostics/sources` section):

```
\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

\<add initializeData="ConnectorsLog"
type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,
Culture=neutral, PublicKeyToken=b77a5c561934e089"
name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,
DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>
```
>[!NOTE]
>If ‘Run this management agent in a separate process’ is enabled, then
`dllhost.exe.config` should be used instead of `miiserver.exe.config`.

**Access token expired error**

Connector might return HTTP error 401 Unauthorized, message “Access token has
expired.”:

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Picture 7. “Access token has expired.” Error

The cause of this issue might be configuration of access token lifetime from the
Azure side. By default, the access token expires after 1 hour. To increase expiration time, please see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Example of this using [Azure AD PowerShell Module Public Preview release](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy -Definition \@('{"TokenLifetimePolicy":{"Version":1,
**"AccessTokenLifetime":"5:00:00"**}}') -DisplayName
"OrganizationDefaultPolicyScenario" -IsOrganizationDefault \$true -Type
"TokenLifetimePolicy"

## Next steps

- [Graph Explorer, great for troubleshooting HTTP call issues]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Versioning, support, and breaking change policies for Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Download Microsoft Identity Manager connector for Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)
[MIM B2B End to End Deployment]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
