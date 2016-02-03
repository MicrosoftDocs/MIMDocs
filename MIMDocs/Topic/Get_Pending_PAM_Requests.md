---
title: Get Pending PAM Requests
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
---
# Get Pending PAM Requests
Used by a privileged account to return a list of pending requests that need approval.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `http://api.contoso.com`.
##Request

Method  |Request URL  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove 

###Query Parameters
Parameter | Description
----------|-------------- 
$filter | Optional. Specify any of the Perding PAM Request properties in a filter expression to return a filtered list of responses. For more information about supported operators, see [Filtering in PAM REST API Service Details](PAM_REST_API_Service_Details.md#Filtering)
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
A successful response contains a list of PAM request approval objects with the following properties.

Property | Description
---------|-------------
RoleName | The display name of the role for which approval is needed.
Requestor | The user name of the requestor to be approved.
Justification | The justification provided by the user.
RequestedTTL | The requested expiration time in seconds. 
RequestedTime | The requested time for elevation.
CreationTime | The creation time of the request.
FIMRequestID | Contains a single element, "Value", with the unique identifier (GUID) of the PAM request.
RequestorID | Contains a single element, "Value", with the unique identifier (GUID) for the Active Directory account that created the PAM request. 
ApprovalObjectID | Contains a single element, "Value", with the unique identifier (GUID) for the Approval Object. 

##Example

###Request
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###Response
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       

