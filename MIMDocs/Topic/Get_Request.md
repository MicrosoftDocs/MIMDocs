---
title: Get Request
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: dcacf36c-0670-44d7-9f40-388667235271
---
# Get Request
Gets one or more specified MIM CM requests.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{id} 

###URL Parameters
Property| Description
---------|--------
id| Optional. The GUID of a request to retrieve.
###Query Parameters
Property| Description
---------|--------
targetuser| Optional. The target user of the request. If no target is specified, all requests, regardless of the target user, will be returned. <br/> **Note**: Only “current” is currently supported.
status| Optional. Indicates the status of the request to retrieve. Possible status types are: *Approved*, *Canceled*, *Completed*, *Denied*, *Executing*, *Failed*, *None*, and *Pending*. <br/>If no status is specified, all requests, regardless of status will be returned. 

###Request Headers
For common request headers, see [HTTP Request and Response Headers](CM_REST_API_Service_Details.md#HttpHeaders) in *CM REST API Service Details*.
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
For common response headers, see [HTTP Request and Response Headers](CM_REST_API_Service_Details.md#HttpHeaders) in *CM REST API Service Details*.
###Response Body
On success, returns one or more [Request](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.requests.request(v=vs.100%29.aspx) objects with the following properties:

Name | Description
-----|------------
Comment | The comment that is associated with the MIM CM request. 
Completed | The time that the MIM CM request was completed. 
DataCollection | The data collection items that are associated with the MIM CM request. 
DataCollectionFlags | The options for data collection for the MIM CM request. 
Flags | The options that are associated with the MIM CM request. 
IsDataCollectionComplete | A Boolean value that indicates if the data collection has been completed for the MIM CM request. 
IsEnrollmentAgent | A Boolean value that indicates if an enrollment agent is required to run the MIM CM request. 
IsSmartcard | A Boolean value that indicates if the MIM CM request is a smart card request or a software profile request. 
NewProfileUuid | The identity of the new software profile that is produced by the MIM CM request. 
NewSmartcardUuid | The identity of the new smart card that is assigned by the MIM CM request. 
OldProfileUuid | The identity of the software profile for which the MIM CM request was created. 
OldSmartcardUuid | The identity of the smart card for which the MIM CM request was created. 
OriginatorUserUuid | The identity of the user who originated the MIM CM request 
Priority | The MIM CM request's priority. 
ProfileTemplateUuid | The identity of the profile template for which the MIM CM request was created. 
RequestType | The type of the MIM CM request. 
SecurityDescriptor | The security descriptor for the MIM CM request. 
Status | The status of the MIM CM request. 
Submitted | The time that the MIM CM request was submitted. 
TargetUserUuid | The identity of the target user for the MIM CM request. 
Uuid | The identifier for the MIM CM request. 

##Example

###Request 1
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1

```
###Response 1
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":3,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"00000000-0000-0000-0000-000000000000",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}```       
###Request 2
```
GET /certificatemanagement/api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc HTTP/1.1
```
###Response 2
```
HTTP/1.1 200 OK

{
    "Uuid":"0c96d73f-967b-420e-854a-43ad2a1504bc",
    "RequestType":5,
    "Status":3,
    "Flags":2,
    "DataCollection":[
        {
            "Name":"Sample Data Item",
            "Value":null
        }
    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:40:33.133Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```       

##See Also

- [Microsoft.Clm.Provision.FindOperations.FindRequest Method](http://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.provision.findoperations.findrequests(v=vs.100%29.aspx)
- [Microsoft.Clm.Shared.RequestPermission Enumeration](http://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.requestpermission(v=vs.100%29.aspx)
- [Microsoft.Clm.Shared.Requests.Request Class](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.requests.request(v=vs.100%29.aspx)
