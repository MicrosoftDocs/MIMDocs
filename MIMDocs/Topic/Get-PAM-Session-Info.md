---
title: Get PAM Session Info
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: bc30e455-9a9c-413a-b8ca-9669286299cf
---
# Get PAM Session Info
Used by a privileged account to get the username of the account that is logged in to the session.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `http://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
GET     |/api/session/sessioninfo 

###Query Parameters
Parameter | Description
----------|-------------- 
v | Optional. The API version. If not included, the current (most recently released) version of the API will be used. For more information, see [Versioning in PAM REST API Service Details](PAM-REST-API-Service-Details.md#Versioning)

###Request Headers
For common request headers, see [HTTP Request and Response Headers](PAM-REST-API-Service-Details.md#HttpHeaders) in *PAM REST API Service Details*.
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
For common response headers, see [HTTP Request and Response Headers](PAM-REST-API-Service-Details.md#HttpHeaders) in *PAM REST API Service Details*.
###Response Body
A successful response returns a PAM session object with the following properties.

Property| Description
--------|-------------
Username| The username of the account that is logged into this session.

##Example

###Request
```
GET /api/session/sessioninfo/ HTTP/1.1
```
###Response
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

