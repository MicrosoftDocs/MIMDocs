---
title: Create Request
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
---
# Create Request
Create a MIM CM request.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests 

###URL Parameters
none

###Request Headers
For common request headers, see [HTTP Request and Response Headers](CM_REST_API_Service_Details.md#HttpHeaders) in *CM REST API Service Details*.
###Request Body
The request body contains the following properties.

Property | Description
---------|-----------
profiletemplateuuid | Required. The GUID of the profile template that the user is creating the request for.
datacollection | Required. A collection of name-value pairs representing the data that is to be provided by the enrollee. The collection of necessary data that must be provided can be retrieved from the profile template’s workflow policy. An empty collection can be specified.
target | Optional. The GUID of the target user that the request is to be created for. If not specified, this defaults to the current user.
type | Required. The type of request that is being created. Available request types are : "Enroll", "Duplicate", "OfflineUnblock", "OnlineUpdate", "Renew", "Recover", "RecoverOnBehalf", "Reinstate", "Retire", "Revoke", "TemporaryCards", and "Unblock".<br/>**Note**: Not all types of request are supported on all profile templates. For example, you can't specify an unblock operation on a software profile template.
comment | Required. Any comments that may be entered by the user. The workflow policy will define whether this is necessary. An empty string can be specified.
priority | Optional. The priority of the request. If not specified, the default request priority, as determined by the profile template settings, will be used.


##Response
###Response Codes
Code  |Description  
---------|---------
201     | Created
403 | Forbidden
500 | Internal Error

###Response Headers
For common response headers, see [HTTP Request and Response Headers](CM_REST_API_Service_Details.md#HttpHeaders) in *CM REST API Service Details*.
###Response Body
On success, returns the URI for the newly created request.
##Example

###Request 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###Response 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
``` 
###Request 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###Response 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       
      
###Request 3
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```
##See Also

- [Microsoft.Clm.Provision.RequestOperations.InitiateEnroll Method](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll(v=vs.100%29.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock Method](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock(v=vs.100%29.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRecover Method](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover(v=vs.100%29.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRetire Method](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire(v=vs.100%29.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateUnblock Method](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock(v=vs.100%29.aspx)
