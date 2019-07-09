---
title: Supported Languages of Microsoft Identity Manager 2016 SP1  | Microsoft Docs
description: A list of languages that are supported by Microsoft Identity Manager 2016 SP1.
keywords:
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/23/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager

ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
---
# Supported languages

This article outlines the supported languages and mapping of updates from Microsoft Identity Manager 2016 SP1 version 4.5.x or greater.

## MIM Service and Portal and Add-ins and Extensions Language Pack 

The Microsoft MIM Service and Portal Language Pack support the following languages 33 languages.  

> [!NOTE]
> In [4.4.1642.0](https://support.microsoft.com/en-us/help/4021562/hotfix-rollup-package-build-4-4-1642-0-is-available-for-microsoft) a registry key was added called "OverrideDefaultUILocale" to MIM Add-ins and Extensions language pack will try to map all similar languages to the one that is supported. For example, if the Windows Display Language is ES-CL (Spanish Chile), or any ES-\*, it will try to map this to ES-ES (Spanish Spain).

> [!IMPORTANT]
> The text in the SSPR add-in and portal will be localized, but the questions will not without additional work. You will need to create AuthN workflows (and accompanying sets and MPRs to target them) to target questions in each language to the target location.

|       Language        | FIM(4.3.x.x)/MIM(4.4.xx) | MIM(4.5.x.x) |
|-----------------------|--------------------------|--------------|
|       Bulgarian       |          bg-BG           |      bg      |
| Chinese (Simplified)  |          zh-CN           |   zh-hans    |
|   Chinese (Taiwan)    |          zh-TW           |   zh-hant    |
|       Croatian        |          hr-HR           |      hr      |
|         Czech         |          cs-CZ           |      cs      |
|        Danish         |          da-DK           |      da      |
|         Dutch         |          nl-NL           |      nl      |
|       Estonian        |          et-EE           |      et      |
|        French         |          fr-FR           |      fr      |
|        Finnish        |          fi-FI           |      fi      |
|        German         |          de-DE           |      de      |
|         Greek         |          el-GR           |      el      |
|         Hindi         |          hi-IN           |      hi      |
|       Hungarian       |          hu-HU           |      hu      |
|        Italian        |          it-IT           |      it      |
|       Japanese        |          ja-JP           |      ja      |
|        Korean         |          ko-KR           |      ko      |
|      Lithuanian       |          lt-LT           |      lt      |
|        Latvian        |          lv-LV           |      lv      |
|       Norwegian       |          nb-NO           |    nb-NO     |
|        Polish         |          pl-PL           |      pl      |
| Portuguese (Portugal) |          pt-PT           |      pt      |
|  Portuguese (Brazil)  |          pt-BR           |    pt-BR     |
|        Russian        |          ru-RU           |      ru      |
|       Romanian        |          ro-RO           |      ro      |
|        Spanish        |          es-ES           |      es      |
|        Slovak         |          sk-SK           |      sk      |
|        Swedish        |          sv-SE           |      sv      |
|       Slovenian       |          sl-SI           |      sl      |
|   Serbian - Serbia    |  sr-latn-CS(Depricated)  |  sr-Latn-RS  |
|         Thai          |          th-TH           |      th      |
|        Turkish        |          tr-TR           |      tr      |
|       Ukrainian       |          uk-UA           |      uk      |

## Certificate Management 
The Microsoft Certificate Management  supports the following 9 languages. 

|Language|FIM(4.3.x.x)/MIM(4.4.xx)|New MIM(4.5.x.x)
|-----|-----|-----|-----|
|Chinese (Simplified)|zh-CN|zh-hans|
|Chinese (Taiwan)|zh-TW|zh-hant|
|Dutch|nl-NL|nl|
|French|fr-FR|fr|
|German|de-DE|de|
|Italian|it-IT|it|
|Japanese|ja-JP|ja|
|Portuguese (Portugal)|pt-PT|pt-PT|
|Spanish|es-ES|es|

## Certificate Management Modern Application  
The Microsoft Certificate Management Modern Application supports the following 33 languages. 

|Language | [1.0.225.104](https://www.microsoft.com/en-us/download/details.aspx?id=54954) | |
|-----|-----|-----|-----|
|Dutch|nl-NL|nl|
|Chinese (Simplified)|zh-CN|zh-hans|
|Chinese (Taiwan)|zh-TW|zh-hant|
|Czech|cs-CZ|cs|
|Danish|da-DK|da|
|French|fr-FR|fr|
|Finnish|fi-FI|fi|
|German|de-DE|de|
|Greek|el-GR|el|
|Hungarian|hu-HU|hu|
|Italian|it-IT|it|
|Japanese|ja-JP|ja|
|Korean|ko-KR|ko|
|Norwegian|nb-NO|nb-NO|
|Polish|pl-PL|pl|
|Portuguese (Portugal)|pt-PT|pt|
|Portuguese (Brazil)|pt-BR|pt-BR|
|Russian|ru-RU|ru|
|Romanian|ro-RO|ro|
|Spanish|es-ES|es|
|Swedish|sv-SE|sv|
|Turkish|tr-TR|tr|

## Next steps

- [First time deployment](microsoft-identity-manager-deploy.md)
- [Version History](reference/version-history.md)
