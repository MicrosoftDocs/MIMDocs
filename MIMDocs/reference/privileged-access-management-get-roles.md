﻿---
# required metadata

title: Get PAM roles | Microsoft Docs
description:
keywords:
author: msmbaldwin
ms.author: barclayn
manager: mbaldwin
ms.date: 09/26/2017
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Get PAM roles
Used by a privileged account to list the PAM roles for which the account is a candidate.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
GET     |/api/pamresources/pamroles

### Query parameters

Parameter | Description
----------|--------------
$filter | Optional. Specify any of the PAM role properties in a filter expression to return a filtered list of responses. For more information about supported operators, see [Filtering in PAM REST API service details](privileged-access-management-rest-api-service-details.md#filtering).
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
A successful response contains a collection of one or more PAM roles, each of which has the following properties:

Property | Description
--------|-------------
RoleID | The unique identifier (GUID) of the PAM role.
DisplayName | THe PAM role’s display name in the MIM service.
Description | The PAM role’s description in the MIM service.
TTL | The role’s access rights maximum expiration timeout in seconds.
AvailableFrom | The earliest time of day when a request is activated.
AvailableTo | The latest time of day when a request is activated.
MFAEnabled | A Boolean value that indicates whether activation requests for this role require an MFA challenge.
ApprovalEnabled | A Boolean value that indicates whether activation requests for this role require approval by a role owner.
AvailabilityWindowEnabled | A Boolean value that indicates whether the role can only be activated during a specified time interval.

## Example
This section provides an example to get the PAM roles.

### Example: Request

```
GET /api/pamresources/pamroles HTTP/1.1
```

### Example: Response

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       
