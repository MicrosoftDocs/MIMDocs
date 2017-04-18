---
# required metadata

title: Function Reference for Microsoft Identity Manager 2016 | Microsoft Docs
description:
keywords:
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/13/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid:

---


# Functions Reference for Microsoft Identity Manager 2016


In Microsoft Identity Manager (MIM) 2016, functions enable you to
modify attribute values prior to flowing them to a target in a function activity
or declarative provisioning. The objective of this document is to give you an
overview of the available functions and a description of how you can use them.

Configuring attribute flow mappings is an elementary task when configuring
synchronization rules. The simplest form of an attribute flow mapping is a
direct mapping. As indicated by the name, a direct mapping takes the value of a
source attribute and applies it to the configured destination attribute. There
are cases where you either need existing attribute values to be modified or new
attribute values to be calculated before the system applies them to a
destination.

Functions are a built-in method used to define the type of modification you need
the synchronization engine to apply when generating an attribute value for a
destination.

In MIM, you can group the existing functions into the following categories:

-   **Data Manipulation Functions**. Functions to perform a variety of manipulation
    operations on strings.

-   **Data Retrieval Functions**. Functions to extract data from attribute values.

-   **Data Generation Functions**. Functions to generate values.

-   **Logic Functions**. Functions to perform operations based on conditions.

The following sections provide more details about the functions in each
category.

## Data manipulation functions

Data manipulation functions are used to perform a variety of manipulation
operations on strings.

| Concatenate        |   |
|--------------------|-------------------------|
| Description        | The Concatenate function is used to concatenate two or more strings.                                                                                                       |
| Function Signature | string1 + string2...                                                                                                                                                     |
| Inputs             | Two or more strings                                                                                                                                                        |
| Operations         | All input string parameters are concatenated with each other.                                                                                                              |
| Output             | One string        |


| UpperCase         |         |
|-------------------|---------|
| Description        | The UpperCase function converts all characters in a string to upper case.         |
| Function Signature | String UpperCase(string)                                                                                                                                                   |
| Inputs             | One string                                                                                                                                                                 |
| Operations         | All lower case characters of the input parameter are converted to upper case characters. Example: UpperCase(“test”) results in “TEST”.                                     |
| Output             | One string                                                              |


| LowerCase          |                                 |
|--------------------|---------------------------------|
| Description        | The LowerCase function converts all characters in a string to lower case.                                                                                                  |
| Function Signature | String LowerCase(string)                                                                                                                                                   |
| Inputs             | One string                                                                                                                                                                 |
| Operations         | All upper case characters of the input parameter are converted to lower case characters. Example: LowerCase(“TeSt”) results in “test”.                                     |
| Output             | One string               |


| ProperCase        |                                                          |
|-------------------|----------------------------------------------------------|
| Description        | The ProperCase function coverts the first character of each space-delimited word in a string to upper case and all other characters are converted to lower case.           |
| Function Signature | String ProperCase(string)                                                                                                                                                  |
| Inputs             | One string                                                                                                                                                                 |
| Operations         | The first character of every space-delimited word in the input parameter is converted to upper case, and all upper case characters are converted to lower case characters. If a word in the input parameter starts with a non-alphabet character, the first character of the word is not converted to upper case. <br/> Examples: <br/> - ProperCase(“TEsT”) results in “Test”. <br/> -   ProperCase(“britta simon”) results in “Britta Simon”. <br/>-   ProperCase(“ TEsT”) results in “ Test”. <br/> -   ProperCase(“\$TEsT”) results in “\$Test”.|
| Output             | One string      |


| LTrim              |      |
|--------------------|------|
| Description        | The LTrim function removes leading white spaces from a string.                                                                                                             |
| Function Signature | String LTrim(string)                                                                                                                                                       |
| Inputs             | One string                                                                                                                                                                 |
| Operations         | The leading white space characters contained in the input parameter are removed. <br/><br/>Example: LTrim(“ Test ”) results in “Test ”.                                              |
| Output             | One string      |



| RTrim              |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Description        | The RTrim function removes trailing white spaces from a string.                                                                 |
| Function Signature | String RTrim(string)                                                                                                            |
| Inputs             | One string                                                                                                                      |
| Operations         | The trailing white space characters contained in the input parameter are removed. Example: RTrim(“ Test ”) results in “ Test”.  |
| Output             | One string                                                                                                                      |


| Trim               |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Description        | The Trim function removes leading and trailing white spaces from a string.                                                      |
| Function Signature | String Trim(string)                                                                                                             |
| Inputs             | One string                                                                                                                      |
| Operations         | The leading and trailing white space characters contained in the string are removed. Example: Trim(“ Test ”) results in “Test”. |
| Output             | One string                                                                                                                      |




| RightPad           |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Description        | The RightPad function right-pads a string to a specified length by using a provided padding character.                          |
| Function Signature | String RightPad(string, length, padCharacter)                                                                                   |
| Operations         | If the length of string is less than length, then padCharacter is repeatedly appended to the end of string until it has a length equal to length. <br/> Examples: <br/> - RightPad(“User”, 10, “0”) would result in “User000000”. <br/> - RightPad(RandomNum(1,10), 5, “0”) could result in “9000”.   |
| Output                                                                                                                                                          | If string has a length greater than or equal to length, a string identical to string is returned. If the length of string is less than length, then a new string of the desired length is returned containing string padded with a padCharacter. If string is null, the function returns an empty string. |   |   |
>[!NOTE]
**padCharacter** can be a space character, but it cannot be a null value. If the length of **string** is equal to or greater than **length**, **string** is returned unchanged.


| LeftPad      |     |
|----|-------|
| Description  | The LeftPad function left-pads a string to a specified length by using a provided padding character.    |
| Function Signature      | String LeftPad(string, length, padCharacter)     |
| Inputs |  - **String.** The string to pad. <br/> - **length.** An integer representing the desired length of string. <br/> - **padCharacter.** A string that consist of a single character to be used as a pad character. |
| Operations  | If the length of string is less than length, then padCharacter is repeatedly appended to the beginning of string until it has a length equal to length. <br/> Examples: <br/> - LeftPad(“User”, 10, “0”) would result in “000000User”. <br/> LeftPad(RandomNum(1,10), 5, “0”) could result in “0009”. |  
|Output | If string has a length greater than or equal to length, a string identical to string is returned. <br/> If the length of string is less than length, then a new string of the desired length is returned containing string padded with a padCharacter. <br/>  If **string** is null, the function returns an empty string.                                                   |

<[!NOTE]
**padCharacter** can be a space character, but it cannot be a null value. If the length of **string** is equal to or greater than **length**, **string** is returned unchanged.

| BitOr    |  |
|----- |------|
| Description  | The BitOr function sets a specified bit on a flag to 1.     |
| Function Signature  | Int BitOr(mask, flag)       |  
| Inputs     | 1. **mask.** A hexadecimal value that specifies the bit to set on the flag. <br/> 2. **flag.** A hexadecimal value that is to have a specific bit modified.    |   
| Operations         | This function converts both parameters to binary representation and compares them: <br/> - Sets a bit to 1 if one or both of the corresponding bits in mask and flag are 1 and to 0 if both of the corresponding bits are 0. <br/> - It returns 1 in all cases except where the corresponding bits of both parameters are 0. <br/> - The resulting bit pattern is the "set" (1 or true) bits of any of the two operands. Multiple flag bits can be set if multiple bits have the value 1 in mask.  |
| Output             | A new version of **flag**, with the bits specified in **mask** set to 1.                   |


| BitAnd             |                                                                                    |
|--------------------|------------------------------------------------------------------------------------|
| Description        | The BitAnd function sets a specified bit on a flag to 0.                           |
| Function Signature | Int BitOr(mask, flag)                                                              |
| Inputs             | 1. **mask.** A hexadecimal value that specifies the bit to modify on the flag. <br/> 2. **flag.** A hexadecimal value that is to have a specific bit modified   |
| Operations         | This function converts both parameters to binary representation and compares them: <br/> - Sets a bit to 0 if one or both of the corresponding bits in **mask** and **flag** are 0 and to 1 if both of the corresponding bits are 1. <br/> - It returns 0 in all cases except where the corresponding bits of both parameters are 1. Multiple flag bits can be set to 0 if multiple bits have the value 0 in **mask.** |
| Output             | A new version of **flag** with the bits specified in **mask** set to 0.                    |

| DateTimeFormat                                 |    |
|---------------------------------------|------------|
| Description       | The DateTimeFormat function is used to format a DateTime in string form to a specified format.     |
| Function Signature   | String DateTimeFormat(dateTime, format)      |
| Inputs   | 1. dateTime. A string representing the DateTime to format.  <br/> 2. **format.** A string representing the format to convert to.  |   
| Operations           | The format string specified in format is applied to the DateTime in the dateTime string. <br/> The string specified in format must be a valid DateTime format. If it is not, an error is returned indicating that the format is not a valid DateTime format. <br/> Example: DateTime(“12/25/2007”, “yyyy-MM-dd”) results in “2007-12-25”.|   
| Output     | A string resulting from applying **format** to **dateTime.**   |

>[!Note]                                                                                                                                                                             
For the characters accepted to create user-defined formats, see [User-Defined Date/Time Formats](http://go.microsoft.com/fwlink/?LinkId=195182)


| ConvertSidToString       |    |   
|--------------------------|----|
| Description       | The ConvertSidToString converts a byte array containing a security identifier to a string.         |
| Function Signature      | String ConvertSidToString(ObjectSID)    |
| Inputs  | **ObjectSID.** A byte array containing a security identifier (SID).   |
| Operations    | The specified binary SID is converted to a string.    |
| Output              | A string representation of the SID.   |  

| ConvertStringToGuid |        |
|---------------------|--------|
| Description         | **The ConvertStringToGuid** function converts the string representation of a GUID to a binary representation of the GUID.      |
| Function            | Byte[] ConvertStringToGuid(stringGuid)  |  
| Inputs              | **Guid.** A string formatted in this pattern: **xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx**, where the value of the GUID is represented as a series of hexadecimal digits in groups of 8, 4, 4, 4, and 12 digits and separated by hyphens. An example of a return value is "382c74c3-721d-4f34-80e557657b6cbc27".  |
| Operations          | The string **Guid** specified in parameter 1 is converted to its binary representation. <br/> If the string is not a representation of a valid **Guid**, the function rejects the argument with the following error: <br/> **The parameter for the ConvertStringToGuid function must be a string representing a valid Guid.**  |
| Output              | A binary representation of the Guid.           |                                                                           


| ReplaceString       |     |
|--------------------|-------|
| Description         | The ReplaceString function replaces all occurrences of a string to another string.  |   
| Function            | String ReplaceString(string, OldValue, NewValue)    |                                                                          
| Inputs              | 1. **String.** A string in which to replace values. <br/> 2. **OldValue.** The string to search for and to replace. <br/> 3. NewValue. The string to replace to. |
| Operations          | All occurrences of OldValue in the string are replaced with NewValue. The function must be able to handle the following special characters: <br/> - **\n.** New Line. <br/> - **\r.** Carriage Return. <br/> - **\t.** Tab. <br/> Example: ReplaceString(“One\n\rMicrosoft\n\r\Way”,”\n\r”,” “) returns “One Microsoft Way”. |   
| Output              | A string with all occurrences of **OldValue** in the string replaced with **NewValue.**      |

## Data retrieval functions

Data retrieval functions are used to perform operations that retrieve the desired characters from a string.

| Word       |        |
|--------------------|---------------|
| Description        | The Word function returns a word contained within a string, based on parameters describing the delimiters to use and the word number to return.                                                                |
| Function Signature | String Word(string, number, delimiters)                                                                                                                                                                        |
| Inputs             | 1. **string.** The string from which to return a word. <br/> 2. **number.** A number identifying which word number should be returned. <br/> 3. **delimeters.** A string representing the delimeters that should be used to identify words. |
| Operations         | Each string of characters in a string that is separated by one of the characters in delimiters is identified as a word. The word that was found at the position specified in parameter 3 (number) is returned: <br/> - If number < 1, return an empty string. <br/> - If string is null, return an empty string. <br/><br/> Examples: <br/> 1. Word(“Test;of%function;”, 3, “;$&%”) returns “function”. <br/> 2. Word(“Test;;Function” , 2 , “;”) returns “” (an empty string). 3. Word(“Test;of%function;”, 0, “;$&%”) returns “” (an empty string).
| Output             | A string containing the word at the position the user asked for. If **string** contains less than number of words, or **string** does not contain any words identified by **delimeters,** an empty string is returned. |  


| Left               |   |
|-------|-------|
| Description        | The Left function returns a specified number of characters from the left of a string.       |
| Function Signature | String Left(string, numChars)     |
| Inputs             | 1. **string.** The string from which to return  characters. 2. **numChars.** A number identifying the number of characters to return from the beginning of a string.         |
| Operations         | **numChars** characters are returned from the first position of the string. <br/> Example: Left(“Britta Simon”, 3) returns “Bri”.   |
| Output             | A string containing the first numChars characters in the string.  <br/> - If numChars = 0, return an empty string. <br/> - If numChars < 0, return an input string. <br/> - If string is null, return an empty string. |




| Right       |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Description | The Right function returns a specified number of characters from the right (end) of a string.                                 |
| Function Signature   | String Right(string, numChars)   |
| Inputs      | 1. **String.** The string from which to return characters. <br/> 2. **numChars.** A number identifying the number of characters to return from the end of string.  |
| Operations  | **numChars.** Characters are returned from the end of a string. <br/> Example: Right(“Britta Simon”, 3) returns “mon”.                  |
| Output      | A string containing the last numChars characters in a string. If numChars = 0, return an empty string. <br/> - If **numChars** < 0, return an input string. <br/> - If the string is null, return an empty string. <br/> - If the string contains fewer characters than the number specified in numChars, a string identical to string is returned. |




| Mid         |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Description | The Mid function returns a specified number of characters from a specified position in a string.                              |
| Function Signature    | String Mid(string, pos, numChars)                                                                                             |
| Inputs      | 1. **string.** The string from which to return characters.   <br/> 2. **pos.** A number identifying the starting position in a string for returning characters. <br/> 3. **numChars.** A number identifying the number of characters to return from a position in the string.  |
| Operations  | Return **numChars** characters starting from position **pos** in the string. <br/>Example: Mid(“Britta Simon”, 3, 5) would return “itta ”. |
| Output      | A string containing **numChars** characters from position **pos** in a string: <br/> - If **numChars** = 0, return an empty string. <br/> - If **numChars** < 0, return an empty string. <br/> - If **pos** > the length of string, return an input string. <br/> - If **pos** ≤ 0, return an input string. <br/> - If **string** is null, return an empty string. <br/> If there are no **numChar** characters remaining in **string** from position **pos**, as many characters as can be returned are returned.

## Data generation functions

Data generation functions are used to generate values for specific data types.

| CRLF               |                                                                                          |
|--------------------|------------------------------------------------------------------------------------------|
| Description        | The CRLF generates a Carriage Return/Line Feed. You use this function to add a new line. |
| Function Signature | String CRLF                                                                              |
| Inputs             | No parameters                                                                            |
| Operations         | A CRLF is returned.                                                                      |
|                    | Example: AddressLine1 + CRLF() + AddressLine2 results in: <br/> - AddressLine1 <br/> - AddressLine2 |
| Output             | A CRLF is the output.                                                                                             |

| RandomNum          |                                                                                                                   |  
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Description        | The RandomNum function returns a random number within a specified interval.                                       |   
| Function Signature | Int RandomNum(start, end)                                                                                         |   
| Inputs             | - **start**. A number identifying the lower limit of the random value to generate.   <br/> - **end**. A number identifying the upper limit of the random value to generate.  |
| Operations         | A random number greater than or equal to **start** and less than or equal to **end** is generated. <br/>  Example: Random(0,999) could return 100.                      |
| Output             | A random number within the range specified by **start** and **end**.                                                      |  

| EscapeDNComponent  |                                                                                                                   |   
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Description        | The *EscapeDNComponent* method processes the input string based on the type of management agent that is being used. |
| Function Signature | String EscapeDNComponent(string)                                                                                  |
| Inputs     | **string**. A string that is used to process a distinguished name. The string should not contain escaped characters. |
| Operations | The EscapeDNComponent method from MIISUtils is used to perform this operation. This method processes the input string based on the type of management agent that is being used. <br/> Because different management agents require different distinguished name formats, this method processes the input strings based upon the type of management agent. The types are Lightweight Directory Access Protocol LDAP) distinguished name, such asActive Directory® Domain Services, Sun Directory Server (formerly iPlanet Directory Server), Microsoft Exchange Server; hierarchical nonLDAP, such as Microsoft Lotus Notes; and extrinsic, such as database and XML without LDAP distinguished names. <br/> **LDAP distinguished name: ** <br/> - Any invalid XML characters in the value portion of a given part are hexadecimal-encoded. <br/>- Any illegal characters (including invalid XML characters) in the name portion of a given part generate an error. <br/> - The following characters are escaped: <br/> &nbsp;&nbsp;&nbsp; - Comma (',') <br/> &nbsp;&nbsp;&nbsp; - Equal sign ('=') <br/> &nbsp;&nbsp;&nbsp; - Plus sign ('+') <br/> &nbsp;&nbsp;&nbsp; - Less-than sign ('<') <br/> &nbsp;&nbsp;&nbsp; - Greater-than sign ('>') <br/> &nbsp;&nbsp;&nbsp; - Number sign ('#') <br/> &nbsp;&nbsp;&nbsp; - Semicolon (';') <br/> &nbsp;&nbsp;&nbsp; - Backslash ('\') <br/> &nbsp;&nbsp;&nbsp; - Quotation mark ('"') <br/> - If the last character in the string is a space, then that space is escaped. <br/> - Any extraneous leading or trailing spaces around a part name are removed. <br/> - For the XML management agent, if there are multiple parts, then the parts are alphabetized. <br/> - If multiple parts are specified, the composite distinguished name string is the concatenation of the individual strings separated by plus signs. <br/> - An error is generated if the input string is not a well-formed, LDAP-style distinguished name string. <br/><br/> **Hierarchical non-LDAP** <br/> - These management agents do not support multipart components. If multiple strings are passed to EscapeDNComponent, an ArgumentException is thrown. <br/> - If any of the characters in the input string are invalid XML characters, an ArgumentException is thrown. <br/> - All commas and backslashes in the input string are escaped. <br/> - If the last character in the string is a space, then that space is escaped. <br/><br/> **Extrinsic:** <br/> 1. If any part is binary or contains an invalid XML character, that part is stored as a hexadecimal-encoded version of the raw data with a '#' character prefixed to the front of the string. For example, if a part was 'AxC' (where x represents an illegal XML character such as '0x10'), that part is encoded as '#410010004300'. <br/> 2. Otherwise, all instances of the following characters are escaped: <br/> &nbsp;&nbsp;&nbsp; - Backslash ('\') <br/> &nbsp;&nbsp;&nbsp; - Comma (',') <br/> &nbsp;&nbsp;&nbsp; - Plus sign ('+') <br/> &nbsp;&nbsp;&nbsp; - Number sign ('#') <br/> 3. If the last character in a given part string is a space, that space is escaped. <br/> 4. If multiple parts are specified, the composite distinguished name string is the concatenation of all the individual strings separated by plus signs.
| Output      | A string containing a valid domain name.                                                                                                                  |   

>[!NOTE]
The validation of distinguished names is less strict than the syntax defined in the LDAP specifications. EscapeDNComponent(String[]) allows a part name to contain any combination of one or more of the characters 'a'-'z', 'A'-'Z', '0'-'9', '-', and '.'. <br/>
It is not possible to specify a binary part with this method. However, it is possible to have a binary part in **CommitNewConnector** if the distinguished name is constructed from anchor attributes and one of the anchor attributes is a binary type.


| Null        |                                                                                                                                                           |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description | The Null function is used to define that this MA does not have an attribute to contribute and that attribute precedence should continue with the next MA. |   
| Function Signature    | String Null    |
| Inputs      | No parameters                                                                                                                                             |   
| Operations  | A Null is returned. <br/> Example: IIF(Eq(domain), “unknown”, Null())                                                                                           |   
| Output      | A Null is output.                                                                                                                                         |   |   |


## Logic Functions
Logic functions are used to perform an operation based on conditions evaluated by the system.

| IIF        |  |
|-------------|---|
| Description | The IIF function returns one of a set of possible values based on a specified condition.    |
| Function Signature   | Object IIF(condition, valueIfTrue, valueIfFalse)   |                                                 |
| Inputs      | 1. **Condition**. Any value or expression that can be evaluated to true or false. 2. **valueIfTrue** A value that is returned if the condition evaluates to true. <br/> 3. **valueIfFalse** A value that is returned if the condition evaluates to false. <br/><br/> The following are functions available for use as expressions in the IIF function as **condition:** <br/> **Eq.** This function compares two arguments for equality. <br/> **NotEquals.** This function compares two arguments for inequality, returning true if they are not equal and false if they are equal.<br/> Example: NotEquals(EmployeeType, "Contractor")<br/> **LessThan.** This function compares two numbers, returning true if the first is less than the second and false otherwise.<br/>Example: LessThan(Salary, 100000) <br/>**GreaterThan.** This function compares two numbers, returning true if the first is greater than the second and false otherwise.<br/> Example: GreaterThan(Salary, 100000) <br/> **LessThanOrEquals.** This function compares two numbers, returning true if the first is less than or equal to the second and false otherwise.<br/>Example: LessThanOrEquals(Salary, 100000) <br/> **GreaterThanOrEquals.* This function compares two numbers, returning true if the first is greater than or is equal to the second and false otherwise. <br/>Example: GreaterThanOrEquals(Salary, 100000)<br/> IsPresent. This function takes as input an attribute in the ILM schema and returns true if the attribute is not null and false if the attribute is null.|
| Operations  | If **condition** evaluates to true, return **valueIfTrue.** Otherwise return **valueIfFalse.** <br/>Example: IIF(Eq(EmployeeType,”Intern”),”t-“ + Alias, Alias)returns the alias of a user with “t-“ added to the beginning of it if the user is an intern. Otherwise, it returns the user’s alias as is. |
| Output      | The output is **valueIfTrue** if the condition is true or **valueIfFalse** if the condition is false. |      
