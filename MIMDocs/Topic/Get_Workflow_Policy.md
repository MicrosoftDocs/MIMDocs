---
title: Get Workflow Policy
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: reference
ms.assetid: be636205-c1f0-457c-982e-e17478cf0889
---
# Get Workflow Policy
Gets the profile template policy for the specified workflow. This data is used during request creation. The workflow policy specifies which data is needed by the client in order to create a request. Such data may include: various data collection items, request comments, and one time password policy.

**Note**: URLs shown in this topic are relative to the hostname chosen during API deployment; for example: `https://api.contoso.com`.
##Request


Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{type} 

###URL Parameters
Parameter| Description
--------|-------------
id| Required. The GUID corresponding to the profile template that the policy is to be extracted from.
type| Required. The type of policy being requested. Possible values are: *Enroll*, *Duplicate*, *OfflineUnblock*, *OnlineUpdate*, *Renew*, *Recover*, *RecoverOnBehalf*, *Reinstate*, *Retire*, *Revoke*, *TemporaryEnroll*, *Unblock*.

###Request Headers
For common request headers, see [HTTP Request and Response Headers](CM_REST_API_Service_Details.md#HttpHeaders) in *CM REST API Service Details*.
###Request Body
none

##Response
###Response Codes
Code  |Description  
---------|---------
200     | OK
403 | Forbidden
204 | No content
500 | Internal Error

###Response Headers
For common response headers, see [HTTP Request and Response Headers](CM_REST_API_Service_Details.md#HttpHeaders) in *CM REST API Service Details*.
###Response Body
On success, returns a policy object based on a [ProfileTemplatePolicy](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy(v=vs.100%29.aspx) object. At a minimum, the policy object will contain the properties in the following table, but may contain additional properties depending on the policy requested. For example, a request for an enroll policy will return an [EnrollPolicy](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy(v=vs.100%29.aspx) object. For more information, see the documentation for the policy object associated with the {type} parameter in the request. The documentation for the different types of policy objects can be found under the [Microsoft.Clm.Shared.ProfileTemplates Namespace](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates(v=vs.100%29.aspx) documentation. 

Property | Description
---------|------------
ApprovalsNeeded | The number of approvals that are required for FIM CM requests for the policy. 
AuthorizedApprover | The security descriptor for users who are authorized to approve FIM CM requests for the policy. 
AuthorizedEnrollmentAgent | The security descriptor for users who can act as enrollment agents for the policy. 
AuthorizedInitiator | The security descriptor for users who can initiate FIM CM requests for the policy. 
CollectComments | A Boolean value that indicates if comment collection is enabled for FIM CM requests for the policy. 
CollectRequestPriority | A Boolean value that indicates if request priority collection is enabled for FIM CM requests for the policy. 
DefaultRequestPriority | The default priority for FIM CM requests for the policy. 
Documents | The policy documents that are configured for the policy. 
Enabled | A Boolean value that indicates if the policy is enabled. 
EnrollAgentRequired | A Boolean value that indicates if enrollment agents are required for FIM CM requests for the policy. 
OneTimePasswordPolicy | Gets how one-time passwords for FIM CM requests for the policy are distributed. 
Personalization | The smart card personalization options for the policy. 
PolicyDataCollection | The data collection items that are associated with the policy. 
SelfServiceEnabled | A Boolean value that indicates if self-service initiation of FIM CM requests is enabled for the policy. 

##Example

###Request 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###Response 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###Request 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###Response 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##See Also

- [Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy Class](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy(v=vs.100%29.aspx)
- [Microsoft.Clm.Shared.ProfileTemplates Namespace](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates(v=vs.100%29.aspx)
