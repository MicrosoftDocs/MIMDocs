---
# required metadata

title: Deprecated Features And Planning For The Future | Microsoft Docs
description: This article documents deprecated features of the MIM Synchronization service still available.
keywords:
author: barclayn
ms.author: davidste
manager: mbaldwin
ms.date: 1/31/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid:

---

# Deprecated Features

This article describes the deprecated features of Microsoft Identity Manager 2016 SP1. Where the feature is still present in Microsoft Identity Manager, it is still supported. Features are not recommended for new deployments, as they may be removed in a feature release.  For developers, we recommend not utilizing deprecated features in any new applications or solutions.

>[!NOTE]
Features and functionalities removed in the Microsoft Identity Manager SP1 are identified with **. <br>
For more information on the support [lifecycle for Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## BHOLD 

Microsoft does not recommend customers start new deployments of the Microsoft BHOLD Suite components. Existing deployments of BHOLD will continue to be supported. Azure AD now provides [access reviews](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) which replace some of the BHOLD attestation campaign features.

## Certificate Management 
| **Category**                | **Deprecated Feature**              | **Replacement and Comment**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Management Agents | **FIM Certificate Management | FIM Certificate Management Agent has been removed in MIM 2016.                                                             |

## Service and Portal

| **Category**                | **Deprecated Feature**              | **Replacement and Comment**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programmatic Configuration | Web Service configuration interface(ma-data and mv-data) | The ability to configure the FIM synchronization service through FIM service web service will be removed in a next version.
|

## Synchronization Service 

| **Category**                | **Deprecated Feature**              | **Replacement and Comment**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Programmatic Configuration | Web Service configuration interface | The ability to configure the FIM synchronization service through the FIM service will be removed in a next version.                                                          |
| Management Agents           | Built-in MAs                        | The following MAs have been removed in MIM 2016: </br> 1.  **MA for FIM Certificate Management </br>2.  **MA for Lotus Notes</br> 3.  **MA for SAP R/3 </br> The Lotus Notes and SAP R/3 MAs have been replaced with new versions. For more information, see [Latest Connector Version Release History & Download](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Management Agents           | ECMA1                               | The ECMA1/XMA extensibility framework has been replaced by the ECMA 2.0. Updating existing ECMA1 management agents with ECMA2.0 connectors is required.                                                                                                                                          |
| Management Agents           | Running Connectors out-of-proc      | This feature will not be replaced. The synchronization service will always call the connector in the same process. It is the responsibility of the connector to start and manage the other process. |
| Management Agents           | Configure partition display name    | This feature will not be replaced. This option was only used to provide an alternative name for a partition in the WMI interfaces.                                                                                                                                                                       |
| Run profiles                | Combined profiles                   | The combined profiles delta import/sync, full import/delta sync, and full import/sync will be removed. You should use run profiles with two steps instead. 

>[!NOTE]
You should keep combined run profiles only in environments where the performance would be impacted by a large number of existing disconnectors.


| **Category**                | **Deprecated Feature**              | **Replacement and Comment**           |
|--------|-------|---|    
| Attribute Precedence | Multi- mastery/equal precedence                       | Equal precedence will be removed. There is no replacement for this feature. You should configure manual precedence instead. You can continue to use this feature if your environment has a FIM Service management agent deployed. This management agent does not provide manual precedence to avoid export-not-precedent for declarative provisioning. |
| Join Rules           | Join on “Any” object type                             | This feature will not be replaced. All join rules should explicitly define the metaverse object type they are trying to join to.       |
| Attribute flows      | Unselect “allow nulls” for exported values            | This feature will not be replaced. “Allow Nulls” will always be selected. You should make sure that you have “Allow Nulls” selected in your current environment.  |
| Attribute flows      | “Do not recall attributes”                            | This feature will not be replaced. Attributes will always be recalled, which is the best practice.  |
| Rules Extension      | Running metaverse and ma rules extension out- of-proc | This feature will not be replaced. The metaverse and attribute flow rules will run in the same process as the synchronization engine.       |
| Rules Extension      | Transaction properties                                | This feature will not be replaced. You should avoid passing data between inbound, provisioning, and outbound synchronization using this utility class.  |
| Rules Extension      | ExchangeUtils: Create55\* methods                     | The methods to create objects for Exchange 5.5 servers will be removed.        |
| Interface            | Mms_Metaverse                                        | All ClmUtils class members will be removed in a next version.   |
