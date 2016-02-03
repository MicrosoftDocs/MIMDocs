---
title: Get PAM Roles
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
---
# Get PAM Roles
Used by a privileged account to list the PAM roles for which the account is a candidate.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `http://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
GET     |/api/pamresources/pamroles

###Query Parameters
Parameter | Description
----------|-------------- 
$filter | Optional. Specify any of the PAM Role properties in a filter expression to return a filtered list of responses. For more information about supported operators, see [Filtering in PAM REST API Service Details](PAM_REST_API_Service_Details.md#Filtering)
v | Optional. The API version. If not included, the current (most recently released) version of the API will be used. For more information, see [Versioning in PAM REST API Service Details](PAM_REST_API_Service_Details.md#Versioning)
###Request Headers
For common request headers, see [HTTP Request and Response Headers](PAM_REST_API_Service_Details.md#HttpHeaders) in *PAM REST API Service Details*.
###Request Body
none

##Response
###Response Codes
Code  |Description  
---------|---------
200 | OK
401 | Unauthorized 
403 | Forbidden 
408 | Request Timeout   
500 | Internal Server Error 
503 | Service Unavailable 

###Response Headers
For common response headers, see [HTTP Request and Response Headers](PAM_REST_API_Service_Details.md#HttpHeaders) in *PAM REST API Service Details*.
###Response Body
A successful response contains a collection of one or more PAM roles, each of which has the following properties.

Property | Description
--------|-------------
RoleID | The unique identifier (GUID) of the PAM role.
DisplayName | THe PAM role’s display name in the MIM service.
Description | The PAM role’s description in the MIM service.
TTL | The role’s access rights maximum expiration timeout in seconds.
AvailableFrom | The earliest time of day that a requst will be activated.
AvailableTo | The latest time of day that a request will be activated.
MFAEnabled | A Boolean value that indicates whether activation requests for this role require an MFA challenge.
ApprovalEnabled | A Boolean value that indicates whether activation requests for this role require approval by a role owner.
AvailibitlyWindowEnabled | A Boolean value that indicates whether the role can only be activated during a specified time interval.

##Example

###Request
```
GET /api/pamresources/pamroles HTTP/1.1
```
###Response
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

