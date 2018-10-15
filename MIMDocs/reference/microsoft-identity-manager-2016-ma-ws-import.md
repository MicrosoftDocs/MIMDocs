---
# required metadata

title: Import Web Services Connector | Microsoft Docs
description: Import Web Services Connector with multiple Web Services configurations in Microsoft Identity Manager.
keywords:
author: fimguy
ms.author: davidste
manager: bhu
ms.date: 12/19/2017
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: 
---

# Import Web Services Connector

When the Import server configuration operation that contains the Web Services management agent (MA) finishes with the error "Unable to retrieve configuration parameters from the extension," the exported configuration files need to be checked.

The error is due to a limitation in the Synchronization Service. The service can't import the server configuration with the Web Services MA and returns the error "Unable to retrieve configuration parameters from the extension." In some cases, the Web Services MA can contain invalid settings that need to be fixed. 

## How to fix the issue

1. Copy the server configuration files into the destination server. 

2. On the destination server, copy the <a href="#web-service-powershell-script">WebServiceMA_apply_before_import_server_configuration.ps1</a> PowerShell script into the folder that contains the exported files for the server configuration.

3. Run the WebServiceMA_apply_before_import_server_configuration.ps1 PowerShell script.

<h3 id="web-service-powershell-script">PowerShell script</h3>

```
# --------------------------------------------------------------------- 
# <copyright file="WebServiceMA_apply_before_import_server_configuration.ps1" company="Microsoft"> 
# Copyright (c). All rights reserved. 
# </copyright> 

# --------------------------------------------------------------------- 
# If the script doesn't work, check the ExecutionPolicy
# by using the following commands: 
# 
#    - Get-ExecutionPolicy 
#    - Set-ExecutionPolicy RemoteSigned 
#

function Get-MIMInstallationPath { 

<# 
  .SYNOPSIS 

  Get the installation path of MIM from the Windows registry.

  .DESCRIPTION 

  Get the installation path of MIM from the Windows registry: 

       HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\
       Forefront Identity Manager\2010\Synchronization Service\ 

  and the paramter Location. 

  .EXAMPLE 

  $path = Get-MIMInstallationPath 
#>   

  try 
  { 
    $MIMpath = Get-ItemProperty -Path "Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Forefront Identity Manager\2010\Synchronization Service\" 
  } 

  catch 
  { 
    $ErrorMessage = $_.Exception.Message 
    throw "Get-MIMInstallationPath exception: $ErrorMessage" 
  } 

  Write-Output "$($MIMpath.Location)Synchronization Service\Extensions\" 

} 

function Get-WsconfigFilesNamesFromExtensionsFolder([string] $path) { 

<# 
  .SYNOPSIS 

  Get the comma-separated list of *.wsconfig files from Extensions folder. 

  .DESCRIPTION 

  Get the comma-separated list of *.wsconfig files from Extensions folder. 

  .PARAMETER path 

  The path to the Extensions folder that contains the *.wsconfig files. 

  .EXAMPLE 

  $wsconfigFiles = Get-WsconfigFilesNamesFromExtensionsFolder("./") 
#>  

 if( [string]::IsNullOrEmpty($path) ) 

  { 
    throw "Get-WsconfigFilesNamesFromExtensionsFolder error: the input variable Path cannot be empty." 
  } 

  try 
  { 
    $files = Get-ChildItem $path -filter "*.wsconfig" -name | ForEach-Object {$_ -replace ".wsconfig", ""} 

    $wsconfigFiles=$files -join "," 
  } 

  catch 
  { 
    $ErrorMessage = $_.Exception.Message 

    Write-Host "Get-WsconfigFilesNamesFromExtensionsFolder exception: $ErrorMessage" 
    Write-Output "" 
  } 

  Write-Output $wsconfigFiles 

} 

function Set-AvailableWebServiceProjects([string] $path) { 

<# 

  .SYNOPSIS 

  Patch exported wsconfig files for Web Service project management agent. 


  .DESCRIPTION 

  The script tries to find the config files of Web Service management agents and tries to fix Web Service project configuration parameter. 

  .PARAMETER path 

  The path to the folder that contains the exported config files of management agents.

  .EXAMPLE 

  $count = Set-AvailableWebServiceProjects("./") 

#>   

  if( [string]::IsNullOrEmpty($path) ) 

  { 
    throw "Set-AvailableWebServiceProjects: path variable is empty" 
  } 

  try 
  { 
    $countPatchedFiles=0 

    $files = Get-ChildItem $path -filter "MA-{*}.xml" -name 

    foreach ($file in $files)  
    { 
      [xml]$xmlConfig = Get-Content $file 

      $connectorName = $xmlConfig."saved-ma-configuration"."ma-data"."ma-listname" 

      if($connectorName -ne "Web Service (Microsoft)" ) 
      { 
        continue 
      } 

      $parameters = $xmlConfig."saved-ma-configuration"."ma-data"."private-configuration"."MAConfig"."parameter-definitions" 
     $target = ($parameters."parameter"|where {$_.name -eq "Web Service project" -and $_.use -eq "connectivity" -and $_.type -eq "drop-down"})          

      if($target -eq $null) 
      { 
        continue 

      } 

     $extensionsFolderPath = Get-MIMInstallationPath 
     $wsconfigFiles = Get-WsconfigFilesNamesFromExtensionsFolder($extensionsFolderPath)

     if($wsconfigFiles -eq '') 
     { 
       continue 
     } 

     $target.validation = [string]$wsconfigFiles   
     $defaultConfig = $wsconfigFiles.Split("{,}") 
     $target."default-value" = $defaultConfig[0] 
     $fileFullPath = "$path$file" 
     Set-ItemProperty $fileFullPath -name IsReadOnly -value $false 
     $xmlConfig.Save($fileFullPath) 
     Set-ItemProperty $fileFullPath -name IsReadOnly -value $true 
     $countPatchedFiles++ 
   } 
  } 

  catch 
  { 
   $ErrorMessage = $_.Exception.Message 

   throw "Set-AvailableWebServiceProjects exception: $ErrorMessage" 
  } 
  Write-Output $countPatchedFiles 
} 

########################################## 
####              Main                #### 
########################################## 

cls 

try 
{ 
  $currentFolder = (Get-Location).path + "\" 

  $countPatchedFiles = Set-AvailableWebServiceProjects($currentFolder) 

  Write-Host "Count of patched Web Service config files:" $countPatchedFiles 
} 
catch 
{ 
  $ErrorMessage = $_.Exception.Message 
  Write-Host "The script was run with excetion: $ErrorMessage" 
} 

[void](Read-Host 'Press Enter to exit…') 
```

## Next steps

- [Install the Web Service Configuration Tool](microsoft-identity-manager-2016-ma-ws-install.md)
- [SOAP deployment guide](microsoft-identity-manager-2016-ma-ws-soap.md)
- [REST deployment guide](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Web Service MA configuration](microsoft-identity-manager-2016-ma-ws-maconfig.md)
