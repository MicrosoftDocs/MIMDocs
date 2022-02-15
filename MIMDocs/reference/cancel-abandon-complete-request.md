---
# required metadata

title: Cancel, abandon, or complete a request | Microsoft Docs
description: Changing the status of a Microsoft Identity Manager (MIM) Certificate Management (CM) request.
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2016
ms.topic: reference
ms.prod: microsoft-identity-manager

ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Cancel, abandon, or complete a request
Mark a Microsoft Identity Manager (MIM) Certificate Management (CM) request as completed, canceled, or abandoned.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
PUT     |/CertificateManagement/api/v1.0/requests/{id}

### URL parameters

Property| Description
---------|--------
id| Required. The GUID of the request to complete.


### Request headers
For common request headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Request body
The request body contains the following properties:

Property | Description
---------|-----------
status | The status to set the request to. The possible values are "Completed," "Canceled," or "Abandoned."


## Response
This section describes the response.

### Response codes

Code  |Description  
---------|---------
200 | OK
204 | No Content
403 | Forbidden
500 | Internal Error

### Response headers
For common response headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Response body
On success, returns a [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) object. The object describes the MIM CM request that has been marked as completed. The object contains the following properties:

Name | Description
-----|------------
Comment | The comment that is associated with the MIM CM request.
Completed | The time that the MIM CM request was completed.
DataCollection | The data collection items that are associated with the MIM CM request.
DataCollectionFlags | The options for data collection for the MIM CM request.
Flags | The options that are associated with the MIM CM request.
IsDataCollectionComplete | A Boolean value that indicates if the data collection has been completed for the MIM CM request.
IsEnrollmentAgent | A Boolean value that indicates if an enrollment agent is required to run the MIM CM request.
IsSmartcard | A Boolean value that indicates if the MIM CM request is a smart card request or a software profile request.
NewProfileUuid | The identity of the new software profile that is produced by the MIM CM request.
NewSmartcardUuid | The identity of the new smart card that is assigned by the MIM CM request.
OldProfileUuid | The identity of the software profile for which the MIM CM request was created.
OldSmartcardUuid | The identity of the smart card for which the MIM CM request was created.
OriginatorUserUuid | The identity of the user who originated the MIM CM request.
Priority | The MIM CM request's priority.
ProfileTemplateUuid | The identity of the profile template for which the MIM CM request was created.
RequestType | The type of the MIM CM request.
SecurityDescriptor | The security descriptor for the MIM CM request.
Status | The status of the MIM CM request.
Submitted | The time that the MIM CM request was submitted.
TargetUserUuid | The identity of the target user for the MIM CM request.
Uuid | The identifier for the MIM CM request.

## Example
This section provides an example to mark a request as completed.

### Example: Request

```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    "status":"Completed"
}
```

### Example: Response

```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":8,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"2015-07-07T23:37:37.767Z",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```

## See also

- [Microsoft.Clm.Provision.ExecuteOperations.Complete method](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft.Clm.Shared.Requests.Request class](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
