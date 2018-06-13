---
# required metadata

title: Get workflow policy | Microsoft Docs
description:
keywords:
author: msmbaldwin
ms.author: barclayn
manager: mbaldwin
ms.date: 09/26/2017
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: be636205-c1f0-457c-982e-e17478cf0889

# optional metadata

#ROBOTS:
audience: developer
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Get workflow policy
Gets the profile template policy for a specified workflow. The data is used during request creation. The workflow policy specifies which data is needed by the client in order to create a request. The data can include various data collection items, request comments, and a one-time password policy.

>[!NOTE]
>The URLs in this article are relative to the hostname that's chosen during API deployment, such as `https://api.contoso.com`.

## Request

Method  |Request URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{type}

### URL parameters

Parameter| Description
--------|-------------
id| Required. The GUID corresponding to the profile template that the policy is to be extracted from.
type| Required. The type of policy that's being requested. The possible values are "Enroll", "Duplicate", "OfflineUnblock", "OnlineUpdate", "Renew," "Recover," "RecoverOnBehalf," "Reinstate," "Retire," "Revoke," "TemporaryEnroll," and "Unblock."

### Request headers
For common request headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Request body
None.

## Response
This section describes the response.

### Response codes

Code  |Description  
---------|---------
200 | OK
403 | Forbidden
204 | No content
500 | Internal Error

### Response headers
For common response headers, see [HTTP request and response headers](certificate-management-rest-api-service-details.md#http-request-and-response-headers) in *CM REST API service details*.

### Response body
On success, returns a policy object that's based on a [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) object. At a minimum, the policy object contains the properties in the following table, but can contain additional properties depending on the policy requested. For example, a request for an enroll policy returns an [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy.aspx) object. For more information, see the documentation for the policy object that's associated with the {type} parameter in the request. The documentation for the different types of policy objects can be found in the [Microsoft.Clm.Shared.ProfileTemplates namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx) documentation.

Property | Description
---------|------------
ApprovalsNeeded | The number of approvals that are required for Forefront Identity Manager (FIM) Certificate Management (CM) requests for the policy.
AuthorizedApprover | The security descriptor for users who are authorized to approve FIM CM requests for the policy.
AuthorizedEnrollmentAgent | The security descriptor for users who can act as enrollment agents for the policy.
AuthorizedInitiator | The security descriptor for users who can initiate FIM CM requests for the policy.
CollectComments | A Boolean value that indicates if comment collection is enabled for FIM CM requests for the policy.
CollectRequestPriority | A Boolean value that indicates if request priority collection is enabled for FIM CM requests for the policy.
DefaultRequestPriority | The default priority for FIM CM requests for the policy.
Documents | The policy documents that are configured for the policy.
Enabled | A Boolean value that indicates if the policy is enabled.
EnrollAgentRequired | A Boolean value that indicates if enrollment agents are required for FIM CM requests for the policy.
OneTimePasswordPolicy | The distribution method for one-time passwords for FIM CM requests for the policy.
Personalization | The smart card personalization options for the policy.
PolicyDataCollection | The data collection items that are associated with the policy.
SelfServiceEnabled | A Boolean value that indicates if self-service initiation of FIM CM requests is enabled for the policy.

## Example
This section provides an example to get the profile template policy for a workflow. 

### Example: Request 1

```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```

### Example: Response 1

```
HTTP/1.1 200 OK

... body coming soon
```       

### Example: Request 2

```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```

### Example: Response 2

```
HTTP/1.1 200 OK

... body coming soon
```       

## See also

- [Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy class](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Microsoft.Clm.Shared.ProfileTemplates namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
