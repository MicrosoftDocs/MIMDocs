---
title: CM REST API Service Details
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
---
# CM REST API Service Details
The following sections discuss details of the Microsoft Identity Manager (MIM) Certificate Management (CM) REST API.

## In This Topic

- [Architecture](#Architecture)
- [HTTP Request and Response Headers](#HttpHeaders)
- [API Versioning](#Versioning)
- [Enabling the API](#APIConfig)
- [Enabling Tracing and Logging](#TracingConfig)
- [Error Handling and Troubleshooting](#ErrorHandling)

<a name="Architecture"></a>
## Architecture 
MIM CM REST API calls are handled by controllers. The following table shows the full list of controllers and samples of the context in which they can be used.

Controller| Sample route
----------|-------------
CertificateDataController| /api/v1.0/requests/{requestid}/certificatedata /
CertificateRequestGenerationOptionsController| /api/v1.0/requests/{requestid}/certificaterequestgenerationoptions
CertificatesController| /api/v1.0/smartcards/{smartcardid}/certificates <br/> /api/v1.0/profiles/{profileid}/certificates
OperationsController| /api/v1.0/smartcards/{smartcardid}/operations <br/> /api/v1.0/profiles/{profileid}/operations
PoliciesController| /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
ProfilesController| /api/v1.0/profiles/{id} <br/> /api/v1.0/Profiles <br/> /api/v1.0/requests/{requestid}/profiles/{id}
ProfileTemplatesController| /api/v1.0/profiletemplates/{id} <br/> /api/v1.0/profiletemplates <br/> /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
RequestsController| /api/v1.0/requests/{id} <br/> /api/v1.0/requests
SmartcardsController| /api/v1.0/requests/{requestid}/smartcards/{id}/diversifiedkey <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/serverproposedpin <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/authenticationresponse <br/> /api/v1.0/requests/{requestid}/smartcards/{id} <br/> /api/v1.0/smartcards/{id} <br/> /api/v1.0/smartcards
SmartcardsConfigurationController| /api/v1.0/profiletemplates/{profiletemplateid}/configuration/smartcards
<br/>
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
## API Versioning 
The current version of the CM REST API is 1.0. The version is specified in the segment directly following the `/api` segment in the URI: `/api/v1.0`. In the future, the version number will change should there be major changes to the API interface.

<a name="APIConfig"></a>
## Enabling the API 
The `<ClmConfiguration>` section of the Web.config file has been extended with a new key:

```
<add key="Clm.WebApi.Enabled" value="false" />
```
<br/>

This key determines whether the CM REST API is exposed to clients. If the key is set to "false", the route mapping for the API is not performed on application startup. This means that any subsequent requests to API endpoints will return an HTTP 404 error code. The key is set to “disabled” by default.
After changing this value to "true", remember to run **iisreset** on the server.

<a name="TracingConfig"></a>
## Enabling Tracing and Logging 
The MIM CM REST API emits trace data for each HTTP request sent to it. You can set the verbosity level of the trace information emitted by setting the following configuration value:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```
<br/>
<a name="ErrorHandling"></a>
## Error Handling and Troubleshooting 
When exceptions occur while processing a request, the MIM CM REST API returns an HTTP status code to the web client. For common errors, the API returns an appropriate HTTP status code and an error code. 

Unhandled exceptions are converted to an `HttpResponseException` with HTTP Status Code 500 (“Internal Error”) and traced in both the event log and the MIM CM trace file. Each unhandled exception is written to the event log with a corresponding correlation ID. The correlation ID is also sent to the consumer of the API in the error message. To troubleshoot the error, an administrator can search the event log for the corresponding correlation ID and error details.

Due to security concerns, stack traces corresponding to errors generated as a result of consuming the MIM CM REST API are not sent back to the client.
