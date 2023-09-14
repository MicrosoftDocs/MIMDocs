---
# required metadata

title: Create request | Microsoft Docs
description: Instructions and examples for creating a MIM CM request.
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 09/14/2023
ms.topic: reference
ms.prod: microsoft-identity-manager

ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Create request
Create a Microsoft Identity Manager (MIM) Certificate Management (CM) request.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests

### URL parameters
None.

### Request headers
For common request headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Request body
The request body contains the following properties:

Property | Description
---------|-----------
profiletemplateuuid | Required. The GUID of the profile template that the user is creating the request for.
datacollection | Required. A collection of name-value pairs representing the data that is to be provided by the enrollee. The collection of necessary data that must be provided can be retrieved from the profile template’s workflow policy. An empty collection can be specified.
target | Optional. The GUID of the target user that the request is to be created for. If not specified, the target defaults to the current user.
type | Required. The type of request that is being created. Available request types include "Enroll," "Duplicate," "OfflineUnblock," "OnlineUpdate," "Renew," "Recover," "RecoverOnBehalf," "Reinstate," "Retire," "Revoke," "TemporaryCards," and "Unblock."<br/><br/>**Note**: Not all request types are supported on all profile templates. For example, you can't specify the Unblock operation on a software profile template.
comment | Required. Any comments that may be entered by the user. The workflow policy defines whether the comment property is necessary. An empty string can be specified.
priority | Optional. The priority of the request. If not specified, the default request priority, as determined by the profile template settings, is used.


## Response
This section describes the response.

### Response codes

Code  |Description  
---------|---------
201 | Created
403 | Forbidden
500 | Internal Error

### Response headers
For common response headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Response body
On success, returns the URI for the newly created request.

## Example
This section provides an example to create enroll and unblock requests.

### Example: Request 1

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```

### Example: Response 1

```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```

### Example: Request 2

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```

### Example: Response 2

```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

### Example: Request 3

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```

## See also

- [Microsoft.Clm.Provision.RequestOperations.InitiateEnroll method](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock method](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRecover method](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRetire method](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateUnblock method](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
