---
# required metadata

title: Get profile templates | Microsoft Docs
description: Using the MIM CM REST API GET command to list profile templates available to a specified user.
keywords:
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager

ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Get profile templates
Gets a list of profile templates for which the specified user can enroll. This method  returns a limited view of the profile template. The profile template data returned should be sufficient to enable the requesting user to decide which profile template, if any, they need to enroll for. If no workflow and permission are specified, all profile templates that are visible to the user are returned.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates?\[targetuser\] 

### URL parameters

Parameter| Description
--------|-------------
targetuser| Optional. Specifies the target user to return profile templates for. If not specified, the identity of the current user is used. <br/><br/>**Note**: Currently, only the current user is supported.

### Request headers
For common request headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Request body
None.

## Response
This section describes the response.

### Response codes

Code  |Description  
---------|---------
200 | OK
204 | No content
500 | Internal Error

### Response headers
For common response headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Response body
On success, returns a list of ProfileTemplateLimitedView objects with the following properties:

Property| Type| Description
--------|-----|--------
Name| string| The display name of the profile template.
Description| string| The description for the profile template.
Uuid| Guid| The identifier for the profile template.
IsSmartcardProfileTemplate| bool| Indicates whether the template is a smart card profile template.
IsVirtualSmartcardProfileTemplate| bool| Indicates whether the profile template requires a virtual smart card.

## Example
This section provides an example to get the list of profile templates for the specified user.

### Example: Request

```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```

### Example: Response

```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]
```       
