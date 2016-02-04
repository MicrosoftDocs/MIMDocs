---
title: Sample Enrollment Walkthrough
ms.custom: 
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
---
# Sample Enrollment Walkthrough
This topic shows the steps required  to perform a Virtual Smartcard Self-Service enrollment. It shows an auto-approved request with a user-set PIN.
1.	The client authenticates the user, then requests a list of profile templates that the authenticated user can enroll for:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates. 
    ```
    <br/>
2.	The client displays the resulting list to the user. The user selects a vSC (Virtual Smart Card) profile template with name “Virtual Smartcard VPN” and UUID *97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1*.

3.	The client retrieves the enrollment policy of the selected profile template by using the UUID returned in the previous step:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policy/workflow/enroll
    ```
 <br/>   
4.	The client analyzes the “DataCollection” field in the returned policy, noting that a single data collection item entitled “Request Reason” appears. The client also notes that the “collectComments” flag is set to *false*, so it doesn’t prompt the user to enter any.

5.	After the user has entered the reason for requiring the certificates, the client creates a request: 

    ```
    POST /CertificateManagement/api/v1.0/requests 
    
    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```
<br/>
6.	The server successfully creates the request and returns the URI of the request to the client: /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5.

7.	The client retrieves the request object by calling the returned URI:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```
<br/> 
8.	The client verifies that the “state” property in the request is set to “approved”. Request execution may begin.

9.	The client examines the request to see if there is a smartcard already associated with the request by analyzing the contents of the “newsmartcarduuid” parameter. 

10.	Since it only contains an empty GUID, the client must either use an existing card not already in use by MIM CM, or create one in the case of the profile template being configured for virtual smartcards. 

11.	Since the latter has been indicated to the client through the initial query for enrollable profile templates (step 1), the client must now create a virtual smartcard device. 

12.	The client retrieves the smartcard policy from the profile template (using the UUID of the template selected in step 3): 

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards 
    ```
13.	The smartcard policy will contain the default admin key for the card in the “DefaultAdminKeyHex” property. When creating the smartcard, the client must set the initial admin key of the smartcard to this key.  

14.	Upon creating the smartcard device, the client must assign it to the request:

    ```
    POST /CertificateManagement/api/v1.0/smartcards 
    
    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
        "atr":"3b8d0180fba000000397425446590301c8"
    }
    ```
<br/>
15.	The server will respond to the client with a URI to the newly created smartcard object: api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644.

16.	The client retrieves the smartcard object:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```
<br/>
17.	By checking the value of the “diversifyadminkey” flag in the smartcard policy obtained in step 12, the client knows it has to diversify the admin key.

18.	The client retrieves the proposed admin key:

    ``` 
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9 
    ```
<br/>
19.	The client must authenticate to the card as admin in order to set the admin key. To do this, the client requests an authentication challenge from the smartcard and submits it to the server:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&atr=AFDEE6&diversified=false
    ```
<br/>
20.	The server sends the challenge response in the HTTP response body and the client uses it to diversify the admin key.

21.	The client notes that the user must provide their desired pin by inspecting the “UserPinOption” field in the smartcard policy. 

22.	After the user has entered their desired pin into a dialog, the client performs a challenge-response authentication as outlined in step 19, with the only difference being that the diversified flag should now be set to “true”:

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&atr=AFDEE6&diversified=true 
    ```
<br/>
23.	The client uses the response received from the server to set the desired user pin.

24.	The client is now ready to generate certificate requests. It queries the profile template certificate generation parameters to determine how the keys/requests should be generated. 

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```
<br/>
25.	The server responds with a single JSON-serialized CertificateRequestGenerationOptions object detailing the exportability of the key, certificate friendly name, hashing algorithm, key algorithm, key size, etc. The client uses these parameters to generate a certificate request.

26.	The single certificate request is submitted to the server. The client could additionally specify a PFX password that should be used to decrypt any PFX blobs in the case where the certificate template specifies archiving the certificates on the CA, i.e. CA generates the key pair, then sends it to the client. The client may also choose to add some comments.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata
    
    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```
<br/>
27.	After a few seconds, the server responds with a single, JSON-serialized Microsoft.Clm.Shared.Certificate object. By checking the “isPkcs7” flag, the client learns that this response is not a PFX blob. The client extracts the blob by base64 decoding the “pkcs7” string and installs it. 

28.	The client marks the request as complete.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5 
    
    {
        "status":"Completed"
    }
    ```
