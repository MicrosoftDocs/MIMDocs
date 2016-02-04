---
title: Get Profile Templates
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
---
# Get Profile Templates
Gets a list of profile templates that the specified user can enroll for. This method  returns a limited view of the profile template. The profile template data returned should be sufficient to enable the requesting user to decide which profile template, if any, they need to enroll for. If no workflow and permission are specified, all profile templates visible to the user will be returned.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates?\[targetuser\] 

###URL Parameters
Parameter| Description
--------|-------------
targetuser| Optional. Specifies the target user to return profile templates for. If not specified, the identity of the current user will be used. <br/>**Note**: Currently, only the current user is supported.

###Request Headers
See the service topic for common request headers
###Request Body
none

##Response
###Response Codes
Code  |Description  
---------|---------
200     | OK
204 | No content
500 | Internal Error

###Response Headers
See the service topic for common response headers.
###Response Body
On success, returns a list of ProfileTemplateLimitedView objects with the following properties.

Property| Type| Description
--------|-----|--------
Name| string| The display name of the profile template.
Description| string| The description for the profile template.
Uuid| Guid| The identifier for the profile template.
IsSmartcardProfileTemplate| bool| Indicates whether the template is a smart card profile template.
IsVirtualSmartcardProfileTemplate| bool| Indicates whether the profile template requires a virtual smart card.

##Example

###Request
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###Response
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
