---
# required metadata

title: Get PAM session info | Microsoft Docs
description: Using the PAM REST API GET command to find the username for the account logged in to a session.
keywords:
author: billmath
ms.author: billmath
manager: amycolannino
ms.date: 09/14/2023
ms.topic: reference
ms.service: microsoft-identity-manager

ms.assetid: bc30e455-9a9c-413a-b8ca-9669286299cf

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Get PAM session info
Used by a privileged account to get the username of the account that is logged in to the session.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
GET     |/api/pamresources/sessioninfo

### Query parameters

Parameter | Description
----------|--------------
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
A successful response returns a PAM session object with the following properties:

Property | Description
--------|-------------
Username | The username of the account that is logged into this session.

## Example
This section provides an example to get the PAM session info.

### Example: Request 

```
GET /api/pamresources/sessioninfo/ HTTP/1.1
```

### Example: Response

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#sessioninfo",
    "value":[
        {
            "Username":"FIMCITONEBOXAD\\Jen"
        }
    ]
}
```       
