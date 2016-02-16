---
title: Get Profile Data
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
---
# Get Profile Data
Gets a list of a user’s software certificate profiles with a list of possible operations that can be performed by the current user. A request can then be initiated for any of the specified operations.

**Note**: The server will set the PIN only if the profile template policy indicates that it should be done. Otherwise, the user should supply it.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles<br/>/CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/api/v1.0/requests/{requestid}/profiles 

###URL Parameters
Parameter | Description
---------|------------
id | The identifier (GUID) of the profile to return.
requestId | The identifier of the request to return the profiles for.

###Query Parameters
Parameter | Description
---------|------------
status | Optional. Indicates the status of the profiles to retrieve data for. Possible status types are: "Active", "Approved", "Canceled", "Completed", "Denied", "Executing", "Failed", "None", and "Pending". <br/>If no status is specified, all profiles, regardless of status will be returned.
###Request Headers
For common request headers, see [HTTP Request and Response Headers](CM-REST-API-Service-Details.md#HttpHeaders) in *CM REST API Service Details*.
###Request Body
none

##Response
###Response Codes
Code  |Description  
---------|---------
200 | OK
204 | No content
403 | Forbidden
500 | Internal Error

###Response Headers
For common response headers, see [HTTP Request and Response Headers](CM-REST-API-Service-Details.md#HttpHeaders) in *CM REST API Service Details*.
###Response Body
On success, returns a list of JSON-serialized [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.profiles.profile(v=vs.100%29.aspx) objects with the following properties:

Property | Description
---------|------------
AssignedUserUuid | The identifier of the user to whom the profile is assigned. 
Comment | The comment that describes the profile. 
Flags | The flags that describe the profile. 
ParentProfileUuid | The identifier of the old profile that the profile has replaced. 
PrimaryProfileUuid | The identifier of the primary profile. 
ProfileOperations | The list of possible operations that can be performed by the current user on the profile.
ProfileTemplateUuid | The identifier of the profile template that contains the policies and settings that govern the profile. 
ProfileTemplateVersion | The version of the profile template at the time that the profile was created. 
Status | The status of the profile. 
Uuid | The profile's identifier. 


##Example

###Request 
```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```
###Response 
```
HTTP/1.1 200 OK

[
    {
        "Uuid":"c0dd5c7d-ec35-4346-baca-3ad711e9722f",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"1c9e2606-fea2-4048-a6ac-b014e54c22df",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"5ad77b40-aa42-4533-9396-c9c59fd021a8",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"ff342953-c444-4dc7-b144-f5515d6460c6",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"1e3a31fe-699b-4a6b-945c-18b83c985bc1",
        "ProfileTemplateVersion":9,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable"
        ]
    }
]
```       
##See Also

- [Microsoft.Clm.Shared.Profiles.Profile Class](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.profiles.profile(v=vs.100%29.aspx)
