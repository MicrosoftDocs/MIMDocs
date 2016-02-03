---
title: Create PAM Request
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
---
# Create PAM Request
Used by a privileged account to elevate to a PAM role.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `http://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
POST     |/api/pamresources/pamrequests 

###Query Parameters
Parameter | Description
--------|-------------
Justification | Optional. The user-supplied reason for the elevation request.
RoleId| Required. The unique identifier (GUID) of the PAM role to elevate to.
RequestedTTL| Required. The requested expiration time, in seconds. 
RequestedTime | Optoinal. The time to elevate privileges.  
v | Optional. The API version. If not included, the current (most recently released) version of the API will be used. For more information, see [Versioning in PAM REST API Service Details](PAM_REST_API_Service_Details.md#Versioning)

**Note**: You can specify the *Justification*, *RoleId*, *RequestedTTL*, and *RequestedTime* parameters as properties in the request body, rather than as query parameters. The *v* parameteer can only be specified as a query parameter.

###Request Headers
For common request headers, see [HTTP Request and Response Headers](PAM_REST_API_Service_Details.md#HttpHeaders) in *PAM REST API Service Details*.
###Request Body
Optional. As noted above, the *Justification*, *RoleId*, *RequestedTTL*, and *RequestedTime* paramters can be specified as properties of a request body instead of specifying them in the URL query string. 

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
A successful response contains a PAM request object with the following properties.

Property | Description
--------|-------------
RequestID | The unique identifier (GUID) for the PAM request.
CreatorID | The unique identifier (GUID) in the MIM service for the account that created the request. 
Justification | The reason for elevation.
DisplayName | The PAM request’s display name in MIM.
CreationTime | The creation time of the request. 
CreationMethod | The method used to create the request. 
ExpirationTime | The expiration time of the request. 
RoleID| The unique identifier (GUID) of the PAM role.
RequestedTTL | The requested expiration timeout in seconds.
RequestedTime | The requested time for elevation.
RequestedStatus | The status of the request. Possible values are: "Active", "Closed", "Closing", "Expired", "Pending Approval", and "Rejected".

##Example

###Request 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###Response 1
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

###Request 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###Response 2
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

