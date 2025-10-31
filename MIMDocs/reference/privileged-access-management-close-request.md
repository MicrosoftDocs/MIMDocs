---
# required metadata

title: Close PAM request | Microsoft Docs
description: Using the PAM REST API POST command to close a request to elevate a role.
keywords:
author: henrymbuguakiarie
ms.author: henrymbugua
ms.date: 04/08/2025
ms.topic: reference
ms.service: microsoft-identity-manager

ms.assetid: ca3a1a68-9a2b-47da-bfc1-eaa360cbe609

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Close PAM request
Used by a privileged account to close a request that it initiated to elevate to a PAM role.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
POST     |/api/pamresources/pamrequests({requestId)/Close

### URL parameters

Parameter | Description
----------|-----------
requestId | The identifier (GUID) of the PAM request to close, specified as `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`.

### Query parameters

Parameter | Description
----------|--------------
v | Optional. The API version. If not included, the current (most recently released) version of the API is used. For more information, see [Versioning in PAM REST API service details](privileged-access-management-rest-api-service-details.md#versioning).

### Request Headers
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
None.

## Example
This section provides an example to close a request.

### Example: Request

```
POST /api/pamresources/pamrequests(guid'5ec10e61-cdd1-404e-a18e-740467d87dbf')/Close HTTP/1.1
```

### Example: Response

```
HTTP/1.1 200 OK
```       
