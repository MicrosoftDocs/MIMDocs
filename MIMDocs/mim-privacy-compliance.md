---
# required metadata

title: Microsoft Identity Manager data handling  | Microsoft Docs
description: Understand Microsoft Identity Manager data handling to indentify and report on data within the enviroment, take action in given system based on operational functions and requirement.
keywords:
author: fimguy
ms.author: davidste
manager: mbaldiwn
ms.date: 05/17/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---
# Microsoft Identity Manager data handling 

Microsoft Identity Manager (MIM) 2016 SP1 enables organizations to manage their users, including their credentials and access, across applications. 

- Synchronization between directories and databases, including AD password sync
- Provisioning with workflow and reports for user, access and credential management
- End user self-service for group management, password reset and account unlock
- Certificate and smartcard lifecycle management

We will go through the fundamentals(search/report/update/delete) of operations decisions your organization will need to practice or impliment across many connected data sources. before deciding on your approach you will need understand the current design and configuration of your identity manager system (MIM). We have outlined a few questions customers will need to identify and answer the following questions: 

- What data do you need and why?
- Where is it stored in synchronization service or MIM service?
- How do will you use this data?
- Are you sharing this data with any third parties data sources(Exporting)
- Who is the Authoritative source for the data and the processing of it?
- What will your data retention and data deletion plan in place?
- Have you identified all the technology you need to process and manage data?

To help you understand your MIM eviroment you can utilize the following tool to document your MIM enviroment 
- [MIM Documentor - Allows to export current configuration](https://github.com/Microsoft/MIMConfigDocumenter)

## Searching for and identifying personal data
Searching data within MIM will be dependant on the configuration and setup. Most enviroments are interconnected but for clarity we will break them out by high level component.

### Syncronization Service

All data in MIM that relates to users is derived from Active Directory (AD) and HR Data sources. When searching for personal data, the first place you should consider searching is AD or connected data sources. 

If your not sure the soure of authority you can track this user from the MIM Syncronization Service Manager console, click the Metaverse Search bar to view the identifiable personal data that is stored in the database. Users can search for a specific user or attribute. 

Clicking on the object will open the user profile page. The object details provide you with the comprehensive details about the object, its attributes, last modified and source of authority, and related connected data source derived from management agent configuration example below.

![mim-privacy-compliance.PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### Service and Portal / PAM
If you have a instance of the Service and Portal or PAM installed being able to search for users is important. 

If you installed the Portal you can use the UI to search on any attribute or query for a particular user

If you ony have the service server(without Portal UI) installed you can run a search syntax based on the [FIMAutomation PSSnapin], Example found [here](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx)

PAM can use the same syntax above or you can use the [MIMPAM Module](https://docs.microsoft.com/en-us/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) specifically the get-pamuser to search for the user within the PAM enviroment.

Other reporting options to search on availible data is in the service and portal
- [Hybrid Reporting](https://docs.microsoft.com/en-us/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Reporting with SCSM](https://docs.microsoft.com/en-us/previous-versions/mim/jj133853%28v%3dws.10%29)

### BHOLD
Bhold core service has a UI that allows you to seach for a user or attributes. 

![bhold search](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

With BHOLD we have a access managment connector for syncronization service you will be able to see the connected user objects and the attributes your sending to BHOLD core.

Also you can load the BHOLD Reporting module

- [BHOLD Reporting](https://docs.microsoft.com/en-us/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### Certificate Managment
Certificate managment service search is built into the UI , The administrator will launch and select the 'Find user and view or manage their information'  

![cm search](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## Exporting personal data
Because the data related to entities in MIM is derived from Multiple sources, Most data is stored in the Synchronization Service database. For this reason, you should export object-related data from MIM Sync or you can determine the owner of this data.

### Syncronization Service
Syncronization service for exporting data simply select the data from the search UI and copy and paste into a csv or preferred format. Another way to export this data is to create a File based MA to drop current data needed about a flagged user of interest. 


### Service and Portal / PAM
Service and portal along with PAM you can export this data run a search syntax based on the [FIMAutomation PSSnapin], Example found [here](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) and pipe it to csv

PAM can use the same syntax above or you can use the [MIMPAM Module](https://docs.microsoft.com/en-us/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) specifically the get-pamuser to search for the user within the PAM enviroment and pipe it to a csv.

- [Example Querying The MIM Service Using PowerShell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### BHOLD
Bhold data can be exported using the bholdreporting module to your perfered format.

### Certificate Managment
Certificate managment date related to personal data is connected to active directory. An administrator can export this data using active directory powershell.

## Updating personal data

Personal data about users or objects in MIM Solutions typically is derived from the user's object in your organization's connected data sources. Because of this, any changes made to the user profile in HR source, or another authoritative system of record, such as AD are then reflected in MIM Synchronization Service.

In order to perform management operations administrators must be part of synchronization operations or admin defined [here](https://docs.microsoft.com/en-us/previous-versions/mim/jj590183(v%3dws.10)).
- Manual review of connectors and metaverse designer
    - This will be opening the synchronization service client
        - Using the metaverse designer
![mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Using the metaverse search
![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)


## Deleting personal data

>[!Note] 
> This article provides guidance on ways to delete personal data from Microsoft Identity Manager and can be used to support your obligations under the GDPR. If you’re looking for general info about GDPR, see the [GDPR section of the Service Trust portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Data in MIM is synced and always updated from its connected data source. When an object is deleted in target, the object's data in MIM can be maintained for purposes of security investigation. This is configured per connected data source rules or rule extentions(code) and/or Object deletion rules.

Articles to help understand options on deleting and updating attributes: 

- [Understanding Deprovisioning](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Using Rules Extensions](https://msdn.microsoft.com/en-us/library/windows/desktop/ms698810(v=vs.100).aspx)
- [MIM Best Practices](https://docs.microsoft.com/en-us/microsoft-identity-manager/mim-best-practices)

It is recommended for the Service & Portal that you keep the default 30 days system resource retention configuration. This tells the service when it will delete not only request data but also any object that needs to be cleared from the system. Once this process occurs all data linked to this object is deleted this includes all SSPR registration data. This plays into the object deletion configuration above

![mim-privacy-compliance-srrc.PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)

An exception to this is certificate management as it will only store the profile uid from certificate services with domain sAMAccountName. Once the user is deleted from AD the user cache is only present for the certificates they have enrolled. We do not recommend deleting anything in the database as this can cause overall harm to the operation of the environment.

## Opt-out of telemetry
Previous builds of MIM build 4.5.x.x used to collects anonymized telemetry about each deployment and transmits this data over HTTPS to Microsoft servers. This data was used by Microsoft to help improve future versions of FIM/MIM in the past.

To disable data collection in previous version run change mode and deselect the following prompt:

![mim-privacy-compliance-ceip.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

or edit the registry and set the value to 0: (Component)CEIP
HKLM\SOFTWARE\Microsoft\Forefront Identity Manager\2010

![mim-privacy-compliance-ceip2.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## Next Steps 
- [For SQL Related privacy Guidance](https://docs.microsoft.com/en-us/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [GDPR section of the Service Trust portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
