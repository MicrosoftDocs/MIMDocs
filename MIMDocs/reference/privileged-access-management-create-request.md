---
# required metadata

title: Create PAM request | Microsoft Docs
description: Using the PAM REST API POST command to create a request to elevate a role.
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/27/2023
ms.topic: reference
ms.prod: microsoft-identity-manager

ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Create PAM request
Used by a privileged account to elevate to a PAM role.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
POST     |/api/pamresources/pamrequests

### Query parameters

Parameter | Description
--------|-------------
Justification | Optional. The user-supplied reason for the elevation request.
RoleId | Required. The unique identifier (GUID) of the PAM role to elevate to.
RequestedTTL | Required. The requested expiration time, in seconds.
RequestedTime | Optional. The time to elevate privileges.  
v | Optional. The API version. If not included, the current (most recently released) version of the API is used. For more information, see [Versioning in PAM REST API service details](privileged-access-management-rest-api-service-details.md#versioning).

>[!NOTE]
>You can specify the **Justification**, **RoleId**, **RequestedTTL**, and **RequestedTime** parameters as properties in the request body, rather than as query parameters. The **v** parameter can only be specified as a query parameter.

### Request headers
For common request headers, see [HTTP request and response headers](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) in *PAM REST API service details*.

### Request body
Optional. The **Justification**, **RoleId**, **RequestedTTL**, and **RequestedTime** parameters can be specified as properties of a request body instead of specifying them in the URL query string.

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
A successful response contains a PAM request object with the following properties:

Property | Description
--------|-------------
RequestID | The unique identifier (GUID) for the PAM request.
CreatorID | The unique identifier (GUID) in the MIM service for the account that created the request.
Justification | The reason for elevation.
CreationTime | The creation time of the request.
CreationMethod | The method used to create the request.
ExpirationTime | The expiration time of the request.
RoleID| The unique identifier (GUID) of the PAM role.
RequestedTTL | The requested expiration timeout in seconds.
RequestedTime | The requested time for elevation.
RequestStatus | The status of the request. The possible values are "Processing," "Active," "Closed," "Closing," "Expired," "PendingApproval," "PendingMFA," and "Rejected."

## Example
This section provides examples to create a PAM request.

### Example: Request 1

```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```

### Example: Response 1

```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

### Example: Request 2

```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```

### Example: Response 2

```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       
