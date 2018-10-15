---
# required metadata

title: Microsoft Identity Manager 2016 Best Practices| Microsoft Docs
description:
keywords:
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/05/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid:

---


# Microsoft Identity Manager 2016 Best Practices

This topic describes the best practices for deploying and operating Microsoft Identity Manager 2016 (MIM)

## SQL setup
> [!NOTE]
> The following recommendations for setting up a server running SQL presume a SQL instance dedicated to the FIMService and a SQL instance dedicated to the FIMSynchronizationService database. If you are running the FIMService in a consolidated environment, you will have to make adjustments appropriate for your configuration.

Configuration of the Structured Query Language (SQL) server is critical to optimal system performance. Achieving optimum MIM performance in large-scale implementations depends on the application of best practices for a server running SQL. For more information, see the following topics about SQL best practices:

-   [Storage Top 10 Best Practices](http://go.microsoft.com/fwlink/?LinkID=183663)

-   [Optimizing tempdb Performance](http://go.microsoft.com/fwlink/?LinkID=188267)

-   [SQL Server Best Practices Article](http://go.microsoft.com/fwlink/?LinkID=188268)

-   [Reorganizing and Rebuilding Indexes](http://go.microsoft.com/fwlink/?LinkID=188269)

### Presize data and log files

Do not rely on autogrow. Instead, manage the growth of these files manually. You can leave autogrow on for safety reasons, but you should proactively manage the growth of the data files. For sample sizes of the MIM database, see the [FIM Capacity Planning Guide](http://go.microsoft.com/fwlink/?LinkID=185246).

### To presize SQL data and log files

1.  Start SQL Server Management Studio.

2.  Navigate to the database FIMService, right-click FIMService, and then click Properties.

3.  On the Files page, expand the database files to the required size.

### Isolate log from data files

Follow the SQL server best practices to isolate the transaction and data log files for the databases onto separate physical disks.

Create additional tempdb files

For optimal performance, we recommend that you create one data file per CPU core in the tempdb file.

### To create additional tempdb files

1.  Start SQL Server Mangement Studio.

2.  Navigate to the database tempdb in System Databases, right-click tempdb, and then click Properties.

3.  On the Files page, create one data file for each CPU core. Be sure to separate the tempdb data and log files to different drives and spindles.

### Ensure adequate space for Log files

It is important to understand your recovery model’s disk requirements. Simple recovery mode may be appropriate during the initial system load to limit the use of your disk space, but the data created after your most recent backup is exposed to data loss. When using Full recovery mode, you need to manage the disk usage through backups which include frequent backups of the transaction log to prevent high disk space usage. For more information, see [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370).

### Limit SQL server memory

Depending on how much memory you have on your SQL server and if you share the SQL server with other services (that is, MIM 2016 Service and MIM 2016 Synchronization Service), you might want to restrict the memory consumption of SQL. You can do this through the following steps.

1. Start SQL Server Enterprise Manager.

2. Select New Query.

3. Run the following query:

   ```SQL
   USE master

   EXEC sp_configure 'show advanced options', 1

   RECONFIGURE WITH OVERRIDE

   USE master

   EXEC sp_configure 'max server memory (MB)', 12000--- max=12G RECONFIGURE
   WITH OVERRIDE
   ```

   This example reconfigures the SQL server to use no more than 12 gigabytes (GB) of memory.

4. Verify the setting by using the following query:

   ```SQL
   USE master

   EXEC sp_configure 'max server memory (MB)'--- verify the setting

   USE master

   EXEC sp_configure 'show advanced options', 0

   RECONFIGURE WITH OVERRIDE
   ```

### Backup and recovery configuration

In general, you should work with your database administrator to design a backup and recovery strategy. Some recommendations include:
- Perform database backups according to your organization’s backup policy. 
- If incremental log backups are not planned, the database should be set to the Simple recovery mode. 
- Ensure that you understand the implications of the different recovery models before implementing your backup strategy. Learn the disk space requirements for these models. Full recovery model requires frequent log backups to avoid high disk space usage. 

For more information, see [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370) and [FIM 2010 Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864).

## Create a Backup Administrator account for the FIM Service after installation

Members of the FIMService Administrators set have unique permissions critical to the operation of your MIM deployment. If you are unable to log on as part of the Administrators set, the only resolution is to roll back to a previous backup of the system. To mitigate this situation, we recommend that you add other users to the FIM Administrative set as part of your post-installation configuration.

## FIM Service


### Configuring FIM Service service Exchange mailbox

The following are best practices for configuring Microsoft Exchange Server for the MIM 2016 Service service account.

- Configure the service account so that it can accept mail only from internal e-mail addresses. Specifically, the service account mailbox should never be able to receive mail from external SMTP servers.

#### To configure the service account

1.  In the Exchange Management Console, select the **FIM Service service account**.

2.  Select Properties, select Mail Flow Settings, and then select **Mail Delivery Restrictions.**

3.  Select the **Require that all senders are authenticated** check box.

For further information, see [Configure Message Delivery Restrictions](http://go.microsoft.com/fwlink/?LinkID=183625).

-   Configure the service account so that it rejects mail with sizes greater than 1 MB. Follow the best practice to [Configure Message Size Limits](http://go.microsoft.com/fwlink/?LinkID=183626) for a Mailbox or a Mail-Enabled Public Folder.

-   Configure the service account so that it has a mailbox storage quota of 5 GB. For optimum results, follow the best practices listed in [Configure Storage Quotas for a Mailbox](http://go.microsoft.com/fwlink/?LinkID=156929).

## MIM Portal


### Disable SharePoint indexing

We recommend that you disable Microsoft Office SharePoint® indexing. There are no documents that need to be indexed. Indexing causes many error log entries and potential performance problems in MIM. To disable SharePoint indexing perform the steps below:

1.  On the server that hosts the MIM 2016 Portal, click Start.

2.  Click All Programs.

3.  In the All Programs list, click Administrative Tools.

4.  Under Administrative Tools, click SharePoint Central Administration.

5.  On the Central Administration page, click Operations.

6.  On the Operations page, under Global Configuration, click Timer job
    definitions.

7.  On the Timer Job Definitions page, click SharePoint Services Search Refresh.

8.  On the Edit Timer Job page, click Disable.

## MIM 2016 Initial Data Load

This section lists a series of steps to increase the performance of the initial data load from external system to MIM. It is important to understand that a number of these steps are only performed during the initial population of the system. They should be reset upon load completion. This is a one-time operation and is not a continuous synchronization.

> [!NOTE]
> For more information about synchronizing users between MIM and Active Directory Domain Services (AD DS), see [How do I Synchronize Users from Active Directory to FIM](http://go.microsoft.com/fwlink/?LinkID=188277) in the FIM documentation.
> 
> [!IMPORTANT]
> Ensure that you have applied the best practices covered in the SQL setup section of this guide. 

### Step 1: Configure the SQL server for initial data load
The initial load of data can be a lengthy process. When you plan to initially load a lot of data, you can shorten the time it takes to populate the database by temporarily turning off full-text search and turning it on again after the export on the MIM 2016 management agent (FIM MA) has completed.

To temporarily turn off full-text search:

1.  Start SQL Server Management Studio.

2.  Select New Query.

3.  Run the following SQL statements:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

> [!IMPORTANT]
> Not implementing these procedures can result in high disk space usage, possibly causing you to run out of disk space. You can find additional details about this topic in [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370). [The FIM Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864) contains additional information.

### Step 2: Apply the minimum necessary MIM configuration during the load process

During the initial load process, you should only apply the minimum configuration required to your FIM configuration for your management policy rules (MPRs) and set definitions. After the data load is completed, create the additional sets required for your deployment. Use the Run-On Policy update setting on the action workflows to apply those policies retroactively on the loaded data.

### Step 3: Configure and populate the FIM Service with external identity data

At this point you should follow the procedures described in the How Do I Synchronize Users from Active Directory Domain Services to FIM guide to configure and synchronize your system with users from Active Directory. If you need to synchronize Group information, the procedures for that process are described in the [How Do I Synchronize Groups from Active Directory Domain Services to FIM](https://technet.microsoft.com/library/ff686936(v=ws.10).aspx) guide.

#### Synchronization and export sequences

To optimize performance, run an export after a synchronization run that results in a large number of pending export operations in a connector space. Then, run a confirming import run on the management agent that is associated with the affected connector space. For example, when you need to run synchronization run profiles on several management agents as part of an initial data load, you should run an export followed by a delta import after each individual synchronization run.
For each source management agent that is part of your initialization cycle, perform the following steps:

1.  Full import on a source management agent.

2.  Full synchronization on the source management agent.

3.  Export on all affected target management agents with staged export
    operations.

4.  Delta import on all affected target management agents with staged export
    operations.

### Step 4: Apply your full MIM configuration

Once your initial data load is completed, you should apply the full MIM configuration for your deployment.

Depending on your scenarios, this may include the creation of additional sets, MPRs, and workflows. For any policies that you need to apply retroactively to all existing objects in the system, use the run-on policy update setting on action workflows to apply those policies retroactively on the loaded data.

### Step 5: Reconfigure SQL to previous settings

Remember to change the SQL setting to its normal settings. This includes:

-   Turning on the full-text search

-   Updating your backup policy per your organization policy

Once you have completed the initial data load, you need to turn on full-text search again. Run the following SQL statements to turn on full-text search again:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

If you have to switch to Simple recovery mode, ensure that you reconfigure your backup schedule in accordance with your organization’s backup policy. Additional details of FIM backup schedules are available in the [FIM Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864).

## Configuration Migration


### Avoid changing display names

For many object types such as MPRs, the syncproduction.ps1 script uses the display name as the only anchor attribute between two systems. Consequently, a change to an existing MPR’s display name results in the deletion of the existing MPR, followed by the creation of a new MPR. This result occurs because the migration process cannot successfully join MPRs whose join criteria have changed. To avoid this issue, you can bind a custom attribute to all configuration object types and use that attribute as the join criteria. This enables you to modify display names without affecting the migration process.

### Avoid changing the content of intermediate files

While the file format and application programming interface (API) of the low-level objects are public and manipulations are supported by developers, we do not recommend that you change the contents of the intermediate formats during the migration. However, it may be necessary to remove entire ImportObjects from changes.xml or to perform find and replace operations on pilot.xml to replace version numbers or pilot Domain Name System (DNS) information for production DNS information.

### Ensure that the version number is correct in pilot.xml when migrating across versions

While migrations across version numbers are not recommended or supported, you can often do this by replacing the pilot version number with the production version number in pilot.xml. Specifically, WorkflowDefinition and

ActivityInformationConfiguration objects require the version number to refer precisely to workflow activities in the production environment. Failing to replace the version number results in the Compare-FIMConfig cmdlet identifying differences between the Extensible Object Markup Language (XOML) attributes on WorkflowDefinitions and migrating the pilot’s version number. The production FIM Service may fail to start workflow activities with the incorrect version number.

### Avoid cyclic references

In general, cyclic references are not recommended in a MIM configuration. However, cycles sometimes occur when Set A refers to Set B and Set B also refers to Set A. To avoid issues with cyclic references, you should change the definition of Set A or Set B so that they both do not refer to each other. Then, restart the migration process. If you do have cyclic references and the Compare-FIMConfig cmdlet results in an error as a result, it is necessary to break the cycle manually. Because the Compare-FIMConfig cmdlet outputs a list of changes in order of precedence, it requires that no cycles exist among the references of configuration objects.

## Security

### MIM MA account

The MIM MA account is not considered a service account and should be a regular user account. The accounts must be able to log on locally in order for the FIM Synchronization Service service account to be able to impersonate it.

To enable the MIM MA account to log on locally

1.  Click Start, click Administrative Tools, and then click Local Security
    Policy.

2.  Open the Local Policies node, and then click User Rights Assignment.

3.  In the policy Allow log on locally, ensure that the FIM MA account is
    explicitly specified, or add it to one of the groups that is already granted
    access.

### FIM Synchronization Service and FIM Services accounts

To configure the servers running the MIM server components in a secure manner, the service accounts should be restricted. Using the previous procedure to turn on the MIM MA account, set the following restrictions on the FIM Synchronization Service and FIM Service accounts:

-   Deny logon as a batch job

-   Deny logon locally

-   Deny access to this computer from the network

The service accounts should not be a member of the local administrators group.

The FIM Synchronization Service service account should not be a member of the security groups used to control access to FIM Synchronization Service (groups starting with FIMSync, for example, FIMSyncAdmins, and so on).

> [!IMPORTANT]
>  If you select the options to use the same account for both service accounts and you separate the FIM Service and the FIM Synchronization Service, then you cannot set Deny access to this computer from the network on the mms Synchronization Service server. If access is denied that would prohibit the FIM Service from contacting the FIM Synchronization Service to change configuration and manage passwords.

### Password reset deployed to kiosk-like computers should set local security to clear virtual memory pagefile

When deploying FIM password reset on a workstation intended to be a kiosk, we recommend that the Shutdown: Clear virtual memory pagefile local security policy setting be turned on to ensure that sensitive information from process memory is not available to unauthorized users.

### Implementing SSL for the FIM Portal

It is highly recommended that you use secure sockets layer (SSL) on the FIM Portal server to secure the traffic between the clients and the server.

To implement SSL:

1.  On the MIM Portal server, open IIS Manager.

2.  Click the local computer name.

3.  Click Server Certificates.

4.  Click Create Certificate Request.

5.  In the Common Name text box, enter the name of the server.

6.  Click Next, and then click Next.

7.  Save the file to any location. You will need to access this location in subsequent steps.

8.  Browse to https://servername/certsrv. Replace servername with the name of the server issuing certificates.

9.  Click Request a new Certificate.

10. Click Submit an Advanced Request.

11. Click Submit a Certificate Request by using a base-64-encoded.

12. Paste the contents of the file that you saved in the previous step.

13. In Certificate Template, select Web Server.

14. Click Submit.

15. Save the certificate to your desktop.

16. In IIS Manager, click Complete Certification Request.

17. Point IIS Manager to the certificate you just saved to the desktop.

18. For Friendly name, type the name of the server.

19. Click Sites, and then select SharePoint – 80.

20. Click Bindings, and then click Add.

21. Select https.

22. For certificate, select the one that has the same name as the server (this is the certificate that you just imported).

23. Click OK.

24. Remove the HTTP binding.

25. Click SSL Settings, and then check Require SSL.

26. Save the settings.

27. Click Start, click Administrative Tools, and then click SharePoint 3.0 Central Administration.

28. Click Operations, and then click Alternate Access Mappings.

29. Click http://servername.

30. Change http://servername to https://servername, and then click OK.

31. Click Start, click Run, type iisreset, and then click OK.

## Performance

For optimal performance configuration:

-   Apply the SQL setup best practices as described in the SQL setup section in this document.

-   Turn off SharePoint Indexing on the MIM Portal site. For more information, see the Disable SharePoint indexing section in this document.

## Feature Specific Best Practices 


### Request Management

By default MIM 2016 purges expired system objects, which includes completed requests with associated approvals, approval responses, and workflow instances on a 30-day interval. If your organization needs a longer request history, you should export requests from MIM and store them in an auxiliary database to preserve them beyond the 30-day window. While the 30-day request deletion window is configurable, extending this window can negatively impact performance due to the additional objects in the system.

### Management Policy Rules

#### Use the appropriate MPR type

MIM provides two types of MPRs, Request and Set Transition:

- Request MPR (RMPR)

  - Used to define the access control policy (authentication, authorization, and action) for Create, Read, Update, or Delete (CRUD) operations against resources.
  - Applied when a CRUD operation is issued against a target resource in MIM.
  - Scoped by the matching criteria defined in the rule, that is, to which CRUD requests the rule applies.

- Set Transition MPR (TMPR)
  - Use to define policies regardless of how the object entered the current state represented by the Transition Set. Use TMPR to model entitlement policies.
  - Applied when a resource enters or leaves an associated set.
  - Scoped to the members of the set.

>[NOTE]
For additional details, see [Designing Business Policy Rules](http://go.microsoft.com/fwlink/?LinkID=183691).

#### Only enable MPRs as necessary

Use the principle of least privilege when applying your configuration. MPRs control the access policy to your MIM deployment. Enable only those features used by most of your users. For example, not all users use MIM for group management, so associated group management MPRs should be disabled. By default, MIM ships with most non-administrator permissions disabled.

#### Duplicate built-in MPRs instead of directly modifying
When needing to modify the built-in MPRs, you should create a new MPR with the required configuration and turn off the built-in MPR. This ensures that any future changes to the built-in MPRs that are introduced through the upgrade process do not negatively impact your system configuration.

#### End-user permissions should use explicit attribute lists scoped to users business needs
Using explicit attribute lists helps to prevent the accidental granting of permissions to non-privileged users when attributes are added to objects. Administrators should explicitly need to grant access to new attributes instead of trying to remove access.

Access to data should be scoped to the business needs of the users. For example, group members should not have access to the filter attribute of the group they are a member of. The filter can inadvertently reveal organizational data that the user would not normally have access to.

#### MPRs should reflect effective permissions in the system
Avoid granting permissions to attributes that the user can never use. For example, you should not grant permission to modify core resource attributes such as objectType. Despite the MPR, any attempt to modify a resource's type after it is created is denied by the system.

#### Read permissions should be separate from Modify and Create permissions when using explicit attributes in MPRs

When explicitly listing attributes in MPRs, the attributes required for Create and Modify are usually different than the ones available for Read. For example, Read can be granted over System attributes such as Creator or objectId, while Create or Modify cannot be specified for System attributes.

#### Create permissions should be separate from Modify permissions when using explicit attributes in rules

The Create operation requires that the user select the objectType as part of its operation. This is a core system attribute that cannot be modified after a Create operation.

#### Use one request MPR for all attributes with the same access requirements

For attributes with the same access requirements that are not expected to change, you can combine them into a single request MPR for efficiency.

#### Avoid giving unrestricted access even to selected principal groups

In MIM, permissions are defined as a positive assertion. Because MIM does not support Deny permissions, giving unrestricted access to a resource complicates providing any exclusions in the permissions. As a best practice, grant only the permissions necessary.

#### Use TMPRs to define custom entitlements

Use Set Transition MPRs (TMPRs) instead of RMPRs to define custom entitlements. TMPRs provide a state-based model to assign or remove entitlements based on the membership in the defined Transition Sets, or roles, and the accompanying workflow activities. TMPRs should always be defined in pairs, one for resources transitioning in, and one for resources transitioning out. In addition, each transition MPR should contain separate workflows for provisioning and deprovisioning activities.

> [!NOTE]
> Any deprovisioning workflow should ensure that the Run On Policy Update attribute is set to true.

#### Enable the Set Transition In MPR last

When creating a TMPR pair, turn on Set Transition In MPR last. This order ensures that no resource is left with the entitlement if it is added to and removed from the set while In MPR is turned on but before Out MPR is turned on.

#### Workflows in TMPR should check target resource state first

Provisioning workflows should first check to determine if the target resource has already been provisioned in accordance with the entitlement. If it has, then it should do nothing.

Deprovisioning workflows should first check to determine if the target resource has been provisioned. If it has, then it should deprovision the target resource. Otherwise, it should do nothing.

#### Select Run On Policy Update for TMPRs

This ensures that the correct provisioning behavior applies when policy updates are implemented and use the RunOn Policy update flag on action workflows associated with the TMPRs. This ensures that changes in the policy definitions apply the action workflows to new members of the Transition Set.

#### Avoid associating the same entitlement with two different Transition Sets

Associating the same entitlement with two different Transition Sets can cause an unnecessary revoking and re-granting of entitlements if the resource moves from one set to the other. As a best practice, ensure that one set contains all resources that require the associated entitlement. This ensures a one-to-one relationship between the Transition Set and the entitlement granting the workflow.

#### Use an appropriate sequence of operations when removing entitlements in the system

The order of the steps performed when removing entitlements in the system can result in two different operational results. Ensure that you understand which order applies to the effect that you want.

To remove an entitlement from the system (and revoke it from all members currently holding the entitlement):

1.  Disable the T-In MPR. This avoids new grants.

2.  Delete the T-Set filter or change it so that the set is empty. This causes all existing members to transition out and applies the transition out policy, including the configured deprovision workflows associated with the entitlement.

3.  Disable the T-Out MPR.

To remove an entitlement but leave the current members alone (for example, stop using MIM to manage the entitlement):

1.  Disable the T-In MPR. This avoids new grants.

2.  Disable the T-Out MPR.

3.  Delete the T-Set filter or change it so that the set is empty. Because the
    set is no longer tied to a TMPR, no deprovision workflows are applied.

### Sets

When applying the best practices for sets, you need to consider the impact of the optimizations on the manageability and ease of future administration. Appropriate testing at expected production scale should be performed to identify the right balance between performance and manageability before applying these recommendations.

>[!NOTE]
> All the following guidelines apply to dynamic sets and dynamic groups.


#### Minimize the use of dynamic nesting

This refers to the filter of a set referencing the ComputedMember attribute of another set. A common reason for nesting sets is to avoid duplicating a membership condition across many sets. While this approach may result in better manageability of the sets, there is a performance tradeoff. You can optimize for performance by duplicating the membership conditions of a nested set instead of nesting the set itself.

You may encounter cases where you cannot avoid nesting sets to satisfy a functional requirement. These are the primary situations where you should nest sets. For example, to define the set of all the groups without Full-Time Employee owners, the nesting of sets must be used as follows: `/Group[not(Owner = /Set[ObjectID = ‘X’]/ComputedMember]`, where ‘X’ is the ObjectID of the set of All Full Time Employees.

#### Minimize the use of negative conditions

Negative conditions are the membership conditions that make use of the following operators or functions: `!=`, `not()`, `\<` , `\<=`. To optimize for performance, where possible, express the condition that you want with multiple positive conditions rather than as a negative condition.

#### Minimize the use of membership conditions based on multivalued reference attributes

The use of conditions based on multivalued reference attributes should be minimized because large numbers of these sets may affect the performance of operations on the attribute used in the membership condition.

### Password Reset

#### Kiosk-like computers that are used for password reset should set local security to clear the virtual memory pagefile

When deploying the MIM password reset on a workstation intended to be a kiosk, we recommend that the Shutdown: Clear virtual memory pagefile local security policy setting be turned on to ensure that sensitive information from the process memory is not available to unauthorized users.

#### Users should always register for a password reset on a computer that they are logged on to

When a user attempts to register for password reset through a Web portal, MIM always initiates registration on behalf of the logged-on user, regardless of who is logged onto the Web site. Users should always register for a password reset on a computer that they are logged on to.

#### Do not set the AvoidPdcOnWan registry key to true

When using MIM 2016 password reset, do not set the AvoidPdcOnWan registry key to true.

If this registry key is set to true, the user very likely goes through the password gates, has their password reset on the primary domain controller (PDC), and attempts to log on. Because of this registry key, the local domain controller does not perform the secondary validation with the PDC, therefore denying the logon request. If the user is denied enough times, they can be locked out of the domain and will need to call support.

#### Do not turn on logging of clear-text passwords

It is possible to log clear-text passwords when turning on diagnostic Service Level tracing in Windows

Communication Foundation (WCF). This option is not turned on by default, and you are discouraged from turning it on in production environments. These passwords are visible as clear-text elements within an encrypted Simple Object Access Protocol (SOAP) message when users register for password reset. For more information, see [Configuring Message Logging](http://go.microsoft.com/fwlink/?LinkID=168572).

#### Do not map an authorization workflow to the password reset process

You should not attach an authorization workflow to a password reset operation. Password reset requires a synchronous response and authorization workflows that contain activities such as the approval activity are asynchronous.

#### Do not map multiple action activities to password reset

You should not attach a workflow that contains more than one action activity to a password reset operation. An example scenario would be attaching a second AD DS password reset activity to a password reset MPR. This scenario is not supported.

#### Require reregistration when adding, removing, or changing the order of activities in an existing workflow

When adding, removing, or changing the order of authentication activities in an existing workflow, always select the option to require re-registration. Users who attempt to authenticate for password reset after an activity has been added to or removed from a workflow, but before they have reregistered, may encounter unwanted effects.

### Portal Configuration and Resource Control Display Configuration

#### Consider adding a privacy disclaimer to the user profile page

In MIM, by default, some user profile information may be displayed to other users. As a courtesy to the users, administrators should consider adding custom text consistent with their company's policies to the User Profile page. For more information about adding custom text to a MIM Portal page, see Introduction to [Configuring and Customizing the FIM Portal](http://go.microsoft.com/fwlink/?LinkID=165848).

### Schema

#### Do not delete Person or Group resource types

Though the Person and Group resource types are not marked as Core resource types, the resources themselves or the attributes assigned to them should not be deleted. The user interface (UI) in the MIM Portal requires that the Person and Group resource types and their attributes are present.

#### Do not modify the core attributes

There are 13 Core attributes assigned to all resource types. You should not in any way modify their relationship to any resource type. The 13 Core attributes are:

-   CreatedTime

-   Creator

-   DeletedTime

-   Description

-   DetectedRulesList • DisplayName

-   ExpectedRulesList

-   ExpirationTime

-   Locale

-   MVObjectID

-   ObjectID

-   ObjectType

-   ResourceTime

Do not delete schema resource with a dependency on auditing requirements

You should not delete your schema resources while you still have auditing requirements for these resources.

#### Making regular expressions case insensitive

In MIM, it can be helpful to make some regular expressions case insensitive. You can ignore case within a group by using ?!:. For example, for Employee Type, use

`\^(?!:contractor\|full time employee)%.`

#### Calculation of the member attribute

The Member attribute exposed to the synchronization engine is actually mapped to ComputedMembers. It is a combination of criteria-based members and manually selected members. Even if you add all three attributes, (Filter, ExplicitMembers, and ComputedMembers), the dynamic calculation of the member attribute does not occur for resource types other than for group and set.

#### Leading and trailing spaces in strings are ignored

In MIM, you can enter strings with leading and trailing spaces, but the MIM system ignores those spaces. If you submit a string with a leading and trailing space, the synchronization engine and Web services ignores those spaces.

#### Empty strings do not equal null

Empty strings are not equal to null in this release of MIM. Empty string input is regarded as a valid value. Not present is regarded as a null.

### Workflow and Request Processing

#### Do not delete default workflows that are shipped with MIM 2016

The following workflows are shipped with MIM and should not be deleted:

-   Expiration Workflow

-   Filter Validation Workflow for Administrators

-   Filter Validation Workflow for Non-Administrators

-   Group Expiration Notification Workflow

-   Group Validation Workflow

-   Owner Approval Workflow

-   Password Reset Action Workflow

-   Password Reset AuthN Workflow

-   Requestor Validation With Owner Authorization

-   Requestor Validation Without Owner Authorization

-   System Workflow Required for Registration

#### Do not run two or more ApprovalActivities in parallel

You should not run two or more ApprovalActivities in parallel. Doing so may cause the request to get stuck in the authorizing phase. For multiple approvals, either include a larger list of approvers in the approval or sequence the two activities back-to-back.

#### Authorization activities should not modify MIM resources data

Avoid using activities that modify the MIM resources, such as the Function Evaluator Activity, as part of the workflows in authorization workflows. Because the request has not been committed while in the authorization point of processing, any modifications performed to the identity information can be applied despite the request being possibly rejected.

### Understanding FIM Service Partitions

The objective of MIM is to process requests that can be initiated by various MIM clients such as the FIM synchronization service and the self-service components according to your configured business policies. By design, each FIM service instance belongs to a logical group that consists of one or more FIM service instances, which is also known as FIM service partition. If you have only one FIM service instance deployed to handle the all requests, it is possible that you experience processing latencies. Some operations can even exceed the default timeout values that are appropriate for self-service operations. FIM service partitions can help you to address this issue.

For additional information see [Understanding FIM Service Partitions](https://social.technet.microsoft.com/wiki/contents/articles/2363.understanding-fim-service-partitions.aspx).

## Next steps
- [FIM Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864)
- [How do I Synchronize Users from Active Directory to FIM](http://go.microsoft.com/fwlink/?LinkID=188277) 
- [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370).