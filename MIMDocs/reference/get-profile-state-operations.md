---
# required metadata

title: Get Profile State Operations | Microsoft Docs
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
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Get Profile State Operations
Gets a list of possible operations that can be performed by the current user on the specified profile. A request can then be initiated for any of the specified operations.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/operations <br/>/CertificateManagement/api/v1.0/smartcards/{id}/operations

###URL Parameters
Parameter | Description
---------|------------
id | The identifier (GUID) of the profile or smartcard.

###Request Headers
For common request headers, see [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API Service Details*.
###Request Body
none

##Response
###Response Codes
Code  |Description  
---------|---------
200     | OK
204 | No content
403 | Forbidden
500 | Internal Error

###Response Headers
For common response headers, see [HTTP Request and Response Headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API Service Details*.
###Response Body
On success, returns a list of possible operations that can be performed by the user on the smartcard. This list may contain any number of the following: *OnlineUpdate*, *Renew*, *Recover*, *RecoverOnBehalf*, *Retire*, *Revoke*, and *Unblock*.

##Example

###Request
```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```
###Response
```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       
