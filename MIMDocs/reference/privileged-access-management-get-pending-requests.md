---
# required metadata

title: Get pending PAM requests | Microsoft Docs
description:
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager

ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Get pending PAM requests
Used by a privileged account to return a list of pending requests that need approval.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

### Query parameters

Parameter | Description
----------|--------------
$filter | Optional. Specify any of the pending PAM request properties in a filter expression to return a filtered list of responses. For more information about supported operators, see [Filtering in PAM REST API service details](privileged-access-management-rest-api-service-details.md#filtering).
v | Optional. The API version. If not included, the current (most recently released) version of the API is used. For more information, see [Versioning in PAM REST API service details](privileged-access-management-rest-api-service-details.md#versioning).

### Request headers
For common request headers, see [HTTP request and response headers](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM REST API service details*.

### Request body
None.

## Response
This section describes the response.

### Response codes

Code  |Description  
---------|---------
200 | OK
401 | Unauthorized
403 | Forbidden
408 | Request Timeout   
500 | Internal Server Error
503 | Service Unavailable

### Response headers
For common request headers, see [HTTP request and response headers](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM REST API service details*.

### Response body
A successful response contains a list of PAM request approval objects with the following properties:

Property | Description
---------|-------------
RoleName | The display name of the role for which approval is needed.
Requestor | The user name of the requestor to be approved.
Justification | The justification provided by the user.
RequestedTTL | The requested expiration time in seconds.
RequestedTime | The requested time for elevation.
CreationTime | The creation time of the request.
FIMRequestID | Contains a single element, "Value," with the unique identifier (GUID) of the PAM request.
RequestorID | Contains a single element, "Value," with the unique identifier (GUID) for the Active Directory account that created the PAM request.
ApprovalObjectID | Contains a single element, "Value," with the unique identifier (GUID) for the Approval Object.

## Example
This section provides an example to get the pending PAM requests.

### Example: Request

```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```

### Example: Response

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       
