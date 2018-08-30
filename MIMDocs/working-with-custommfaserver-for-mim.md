---
title: Use Alternant Multi-Factor Authentication API to activate PAM or SSPR Scenarios | Microsoft Docs
description: Set up Custom MFA API as a second layer of security when your users activate roles in Privileged Access Management and Self Service Password Reset.
keywords:
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 08/30/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
---

# Use Custom Multi-Factor Authentication API to activate PAM or SSPR
> [!IMPORTANT]
> Due to the announcement of Deprecation of Azure Multi-Factor Authentication Software Development Kit. The Azure MFA SDK will be supported for existing customers up until the retirement date of November 14, 2018. New customers and current customers will not be able to download SDK anymore via the Azure classic portal. To download you will need to reach out to Azure customer support to receive your generated MFA Service Credentials package. <br> The Microsoft development team is working on changes to MFA by integrating with Azure Multi-Factor Authentication Server SDK.

The Article below will outline the alternant option to enable custom Multi-Factor Authentication Server SDK when released as this will be included in an upcoming hotfix please see [version history](/reference/version-history.md) for announcements. 

## Prerequisites

In order to use custom Multi-Factor Authentication API with MIM, you need:

- Phone numbers for all candidate users
- MIM hotfix [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) 
 
) or greater see [version history](/reference/version-history.md) for announcements

## Sample Custom Authentication Server Code

### Step 1: Patch Server to 4.5.202.0
Download and install MIM hotfix [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) or greater.

### Step 2: Sample Code
```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using Microsoft.IdentityManagement.PhoneServiceProvider;

namespace CustomPhoneGate
{
    public class CustomPhoneGate: IPhoneServiceProvider
    {
        string path = @"c:\Test\phone.txt";
        public PhoneCallStatus GetCallStatus(string callId)
        {
            int res = 2;
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        bool b = Int32.TryParse(info[2], out res);
                        if (!b)
                        {
                            res = 2;
                        }
                    }
                    break;
                }
            }
            switch(res)
            {
                case 0:
                    return PhoneCallStatus.Pending;
                case 1:
                    return PhoneCallStatus.Success;
                case 2:
                    return PhoneCallStatus.Failed;
                default:
                    return PhoneCallStatus.Failed;
            }       
        }
        public string GetFailureMessage(string callId)
        {
            string res = "Call ID is not found";
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        res = info[3];
                    }
                    else
                    {
                        res = "Description is not found";
                    }
                    break;
                }
            }
            return res;            
        }
        
        public PhoneCallStatus InitiateCall(string phoneNumber, Guid requestId, Dictionary<string,object> deliveryAttributes)
        {
            // Here should be some logic for performing voice call
            // For testing purposes we just write details in file             
            string info = string.Format("{0};{1};{2};{3}", requestId, phoneNumber, 0, string.Empty);
            using (StreamWriter sw = File.AppendText(path))
            {
                sw.WriteLine(info);                
            }
            return PhoneCallStatus.Pending;    
        }
    }
}
```
### Step 3: Backup and Open the MfaSettings.xml located in the "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### Step 4: Update / clear the following lines
1. Remove/Clear all configuration entries lines <br>

1. Update or add the following lines to the following to MfaSettings.xml with your custom phone provider <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`
3. Restart MIM Service and test Functionality with Azure Multi-Factor Authentication Server.
>[!NOTE] To revert setting replace MfaSettings.xml with your backup file in step 2


## Next Steps

-    [Getting started with the Azure Multi-Factor Authentication Server](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [What is Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [MIM version release history](./reference/version-history.md)