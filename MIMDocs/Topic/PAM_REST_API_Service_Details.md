---
title: PAM REST API Service Details
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
---
# PAM REST API Service Details
The following sections discuss details of the Microsoft Identity Manager (MIM) Privileged Access Management (PAM) REST API.

<a name="HttpHeaders"></a>
## HTTP Request and Response Headers

HTTP Requests sent to the API should include the following headers (this list is not exhaustive):

Header | Description
-------|------------
Authorization | Required. The contents will depend on the authentication method, which is configurable and can be based on WIA (Windows Integrated Authentication) or ADFS.
Content-Type | Required if the request has a body. Must be "application/json".
Content-Length | Required if the request has a body. 
Cookie | The session cookie. May be required depending on authentication method.
<br/>
HTTP Responses will include the following headers (this list is  not exhaustive):

Header | Description
-------|------------
Content-Type | The API always returns "application/json".
Content-Length | The length of the request body, if present, in bytes.

<a name="Versioning"></a>
## Versioning 
The current version of the API is 1. 
The API version can be specified through a query parameter in the request URL, as in the following example: `http://localhost:8086/api/pamresources/pamrequests?v=1`
If the version is not specified in the request, the request is executed against the most recently released version of the API. 

## Security 
Access to the API requires Integrated Windows Authentication (IWA). This should be configured manually in IIS before Microsoft Identity Manager (MIM) installation.

HTTPS (TLS) is supported, but should be configured manually in IIS. For information, see: **Implement Secure Sockets Layer (SSL) for the FIM Portal** in [Step 9: Perform FIM 2010 R2 Post-Installation Tasks](https://technet.microsoft.com/en-us/library/hh322875(v=ws.10%29.aspx) in the Installing FIM 2010 R2 Test Lab Guide. 

You can generate a new SSL server certificate by running the following command at the Visual Studio Command Prompt:
```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
This command creates a self-signed certificate that can be used to test a web application that uses SSL on a web server whose URL is test.cwap.com. The OID defined by the -eku option identifies the certificate as an SSL server certificate. The certificate is stored in the my store and is available at the machine level, so you can export it from the Certificates snap-in in mmc.exe

## Cross Domain Access (CORS) 
CORS is supported, but should be configured manually in IIS. Add the following elements to the deployed API web.config file to configure the API to allow cross domain calls: 

```
<system.webServer>       
    <httpProtocol> 
        <customHeaders> 
            <add name="Access-Control-Allow-Credentials" value="true"  /> 
            <add name="Access-Control-Allow-Headers" value="content-type" /> 
            <add name="Access-Control-Allow-Origin" value="http://<hostname>:8090" /> 
        </customHeaders> 
    </httpProtocol> 
</system.webServer> 
```
<br/>
<a name="ErrorHandling"></a>
## Error Handling 
The API returns HTTP error responses to indicate error conditions. Errors are OData compliant. The following table shows the error codes that may be returned to a client.

HTTP Status Code | Description
-----------------|------------
401 | Unauthorized 
403 | Forbidden 
408 | Request Timeout   
500 | Internal Server Error 
503 | Service Unavailable 
<br/>
<a name="Filtering"></a>
## Filtering 
PAM REST API requests can include filters to specify the properties that should be included in the response. Filter syntax is based on OData expressions.

Filters can specify any of the properties of PAM Requests, PAM Roles. or Pending PAM Requests. For example: *ExpirationTime*, *DisplayName*, or any other valid property of a PAM Request, PAM Role, or Pending Request.

The API supports the following operators in filter expressions: *And*, *Equal*, *NotEqual*, *GreaterThan*, *LessThan*, *GreaterThenOrEqueal*, and *LessThanOrEqual*. 

The following sample requests include filters:

- This request returns all the PAM Requests between specific dates: `http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime"2015-01-09T08:26:49.721Z" and ExpirationTime lt datetime"2015-02-10T08:26:49.722Z" `
 
- This request returns the Pam Role with the display name "SQL File Access": `http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq "SQL File Access" `
