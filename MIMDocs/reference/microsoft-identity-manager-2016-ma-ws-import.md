---
# required metadata

title: Microsoft Identity Manager Importing Web Services Connector | Microsoft Docs
description: Microsoft Identity Manager Importing Web Services Connector with multiple ws configurations
keywords:
author: fimguy
ms.author: davidste
manager: bhu
ms.date: 12/19/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
---

# Importing Web Services Connector

In case when the “Import server configuration” operation which contains “Web Service management agent” has finished with an error “Unable to retrieve configuration parameters from the extension”, it is necessary to check the exported configuration files.

It happens because of limitation in "Synchronization Service", which cannot import server configuration with "Web Service management agent" and shows an error "Unable to retrieve configuration parameters from the extension". The configuration of "Web Service management agent" in some cases can contain invalid setting which it is necessary to fix. 

# How to fix this issue

Copy server configuration files into the destination server. 

On the destination server copy "WebServiceMA_apply_before_import_server_configuration.ps1" PowerShell script (is placed in Appendix A) into the folder which contains exported files of "Server configuration". 
 
Run "WebServiceMA_apply_before_import_server_configuration.ps1" PowerShell script. 

```
# ------------------------------------------------------------------------------------------------- 
# <copyright file="WebServiceMA_apply_before_import_server_configuration.ps1" company="Microsoft"> 
# Copyright (c) . All rights reserved. 
# </copyright> 

# ------------------------------------------------------------------------------------------------- 
# If the script does not work, it is necessary to check the ExecutionPolicy with the help of 
# the following commands: 
# 
#    * Get-ExecutionPolicy 
#    * Set-ExecutionPolicy RemoteSigned 

function Get-MIMInstallationPath { 

<# 

  .SYNOPSIS 

  Get instalation path of MIM from the Windows registry. 

   

  .DESCRIPTION 

  Get instalation path of MIM from the Windows registry 

  HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Forefront Identity Manager\2010\Synchronization Service\ 

  the paramter Location. 

   

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

  Get the comma-separated list of  *.wsconfig files from Extensions folder. 

   

  .DESCRIPTION 

  Get the comma-separated list of  *.wsconfig files from Extensions folder. 

   

  .PARAMETER path 

  The path to the Extensions folder which is contained *.wsconfig files. 

   

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

  The script tries to found the config files of Web Service management agents 

  and tries to fix Web Service project configuration parameter. 

   

  .PARAMETER path 

  The path to the folder which is contained exported config files of management agents. 

   

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

     ################## 

 ####      Main      #### 

   ################## 

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



## Next Steps 

-   [Install the Web Service Config Tool](microsoft-identity-manager-2016-ma-ws-install.md)

-   [Soap Based deployment guide](microsoft-identity-manager-2016-ma-ws-soap.md)

-   [Rest Based deployment guide](microsoft-identity-manager-2016-ma-ws-restgeneric.md)

-   [Web Service MA Configuration](microsoft-identity-manager-2016-ma-ws-maconfig.md)
