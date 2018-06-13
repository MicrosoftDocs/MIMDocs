---
# required metadata

title: MIM Service Dynamic Logging | Microsoft Docs
description: Enable MIM service dynamic logging without having to restart the management service
keywords:
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid:

# optional metadata


---
# MIM SP1 (4.4.1436.0)  Service Dynamic Logging
In 4.4.1436.0 We have introduced a new logging capability. This enable administrator and support engineers to turn on logging without having to restart the management service.

Once installed you will see the following new line in the Microsoft.ResourceManagement.Service.exe.config  called

*	Line 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*	Line 8:  ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*	Line 266 ``</system.diagnostics> ``

![Highlighted sections showing the new dynamic logging entries](media/mim-service-dynamic-logging/screen01.png)

Logging levels of the dynamic can be found [here](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3)

- Critical = default level service will write only critical events
- Update Line 8 (dynamicLogging mode="true" loggingLevel="Critical") with preferred logging value

Dynamic logging config located at line 266 : Microsoft.ResourceManagement.Service.exe.config

![Highlighted sections showing lines with the various logging areas available](media/mim-service-dynamic-logging/screen02.png)

By default the logging location will be at the **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service** , The FIM Service account will need write permission to this location to generate the dynamic log.

![Folder location of the logs](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  In case of unexpected errors (syntax errors in config file Microsoft.ResourceManagement.Service.exe.config or other mistakes) corresponding error message will be written into file Microsoft.ResourceManagement.Service.exe_Emergency.log under following path %TMP% or %TEMP% or %USERPROFILE% (first that exists).  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

To view the trace you can use the [Service Trace viewer tool](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Service trace viewer screenshot](media/mim-service-dynamic-logging/screen04.png)
