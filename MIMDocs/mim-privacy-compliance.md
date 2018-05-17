---
# required metadata

title: Microsoft Identity Manager compliance, trust, data security, and privacy | Microsoft Docs
description: Understand Microsoft Identity Manager compliance, trust, data security, and privacy
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
# Microsoft Identity Manager compliance, trust, data security, and privacy

>[!Note] 
> This article provides steps for how to delete personal data from the device or service and can be used to support your obligations under the GDPR. If you’re looking for general info about GDPR, see the [GDPR section of the Service Trust portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

The GDPR obligations include discovering what personal data you hold and where it resides, controlling how your users access and use data. The capabilities available today in Microsoft Identity Manager can help you on your journey to searching and reducing risks to achieving compliance with the GDPR. A key requirement of the GDPR is protecting personal data.

## Searching for and identifying personal data
All data in MIM that relates to entities is derived from Active Directory (AD) and HR Data sources. When searching for personal data, the first place you should consider searching is AD or connected data sources.

From the MIM, use the search bar to view the identifiable personal data that is stored in the database. Users can search for a specific user or attribute. Clicking on the object will open the user profile page. The object details provide you with the comprehensive details about the object, its attributes, last modified and source of authority, and related connected data source derived from management agent configuration.

![mim-privacy-compliance.PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)


## Updating personal data
Personal data about users and entities in MIM Solutions typically is derived from the user's object in your organization's connected data sources. Because of this, any changes made to the user profile in HR\AD are reflected in MIM Synchronization Service.

Fist step to updating any data you will need to understand the current design and configuration of your identity manager system (FIM/MIM). We have outlined a few questions customers will need to identify and answer the following questions: 

- What data do you need and why?
- Where is it stored in synchronization service or MIM service?
- How do will you use this data?
- Are you sharing this data with any third parties data sources(Exporting)
- Who is the Authoritative source for the data and the processing of it?
- What will your data retention and data deletion plan in place?
    - Synchronization Service Object Deletion rules
    - Service and Portal Retention Policy
- Have you identified all the technology you need to process and manage data?

In order to perform management operations administrators must be part of synchronization operations or admin defined [here](https://docs.microsoft.com/en-us/previous-versions/mim/jj590183(v%3dws.10)).
- [MIM Documentor - Allows to export current configuration](https://github.com/Microsoft/MIMConfigDocumenter)
- Manual review of connectors and metaverse designer
    - This will be opening the synchronization service client
        - Using the metaverse designer
![mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Using the metaverse search
![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)


## Deleting personal data
Data in MIM is synced and always updated from its connected data source. When an object is deleted in target, the object's data in MIM can be maintained for purposes of security investigation. This is configured per connected data source rules or rule extentions(code) and/or Object deletion rules.

Articles to help you understand options: 

- [Understanding Deprovisioning](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Using Rules Extensions](https://msdn.microsoft.com/en-us/library/windows/desktop/ms698810(v=vs.100).aspx)
- [MIM Best Practices](https://docs.microsoft.com/en-us/microsoft-identity-manager/mim-best-practices)

It is recommended for the Service & Portal that you keep the default 30 days system resource retention configuration. This tells the service when it will delete not only request data but also any object that needs to be cleared from the system. Once this process occurs all data linked to this object is deleted this includes all SSPR registration data. This plays into the object deletion configuration above

![mim-privacy-compliance-srrc.PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)

An exception to this is certificate management as it will only store the profile uid from certificate services with domain sAMAccountName. Once the user is deleted from AD the user cache is only present for the certificates they have enrolled. We do not recommend deleting anything in the database as this can cause overall harm to the operation of the environment.

## Exporting personal data
Because the data related to entities in MIM is derived from Multiple sources, Most data is stored in the Synchronization Service database. For this reason, you should export object-related data from MIM Sync or determine the owner of this data.

We have other options as it relates to reporting on any data to search that is in the Service and portal or Bhold.

- [Hybrid Reporting](https://docs.microsoft.com/en-us/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Reporting with SCSM](https://docs.microsoft.com/en-us/previous-versions/mim/jj133853%28v%3dws.10%29)
- [BHOLD Reporting](https://docs.microsoft.com/en-us/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

## Opt-out of telemetry
Previous to MIM build 4.5.x.x used to collects anonymized telemetry about each deployment and transmits this data over HTTPS to Microsoft servers. This data was used by Microsoft to help improve future versions of FIM/MIM in the past.

For more information, see Manage telemetry settings.

To disable data collection in previous version run change mode and deselect the following prompt:

![mim-privacy-compliance-ceip.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

or edit the registry and set the value to 0: (Component)CEIP
HKLM\SOFTWARE\Microsoft\Forefront Identity Manager\2010

![mim-privacy-compliance-ceip2.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## Next Steps 

- [GDPR section of the Service Trust portal](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
