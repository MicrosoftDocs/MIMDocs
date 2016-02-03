---
title: Approve or Reject a Pending PAM Request
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: 0632656f-ecf4-4090-85a8-216d5638140a
---
# Approve or Reject a Pending PAM Request
Used by a privileged account to approve, close, or reject a request to elevate to a PAM role.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `http://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
POST     |/api/pamresources/pamrequeststoapprove({approvalId)/Approve <br/>/api/pamresources/pamrequeststoapprove({approvalId)/Reject 

###URL Parameters
Parameter | Description
----------|-----------
approvalId | The identifier (GUID) of the approval object in PAM specified as follows `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

###Query Parameters
Parameter | Description
----------|-------------- 
v | Optional. The API version. If not included, the current (most recently released) version of the API will be used. For more information, see [Versioning in PAM REST API Service Details](PAM_REST_API_Service_Details.md#Versioning)


###Request Headers
For common request headers, see [HTTP Request and Response Headers](PAM_REST_API_Service_Details.md#HttpHeaders) in *PAM REST API Service Details*.
###Request Body
none.

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
none
##Example

###Request
```
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1


```
###Response
```
HTTP/1.1 200 OK

```       

