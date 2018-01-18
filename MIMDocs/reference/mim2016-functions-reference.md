﻿---
# required metadata

title: Functions reference for Microsoft Identity Manager 2016 | Microsoft Docs
description:
keywords:
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/26/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid:

---


# Functions reference for Microsoft Identity Manager 2016

In Microsoft Identity Manager (MIM) 2016, functions enable you to modify attribute values prior to flowing them to a target in a function activity or declarative provisioning. The objective of this document is to give you an overview of the available functions and a description of how you can use them.

Configuring attribute flow mappings is an elementary task when configuring synchronization rules. The simplest form of an attribute flow mapping is a direct mapping. As indicated by the name, a direct mapping takes the value of a source attribute and applies it to the configured destination attribute. There are cases where you either need existing attribute values to be modified or new attribute values to be calculated before the system applies them to a destination.

Functions are a built-in method used to define the type of modification you need the synchronization engine to apply when generating an attribute value for a destination.

The MIM functions are distributed into the following categories:

- Data manipulation: Perform a variety of manipulation operations on strings.

- Data retrieval: Extract data from attribute values.

- Data generation: Generate values.

- Logic: Perform operations based on conditions.

The following sections provide more details about the functions in each category.


## Data manipulation functions

Data manipulation functions are used to perform a variety of manipulation operations on strings.

| Concatenate             |   |
|---|---|
| Description             | The Concatenate function concatenates two or more strings. |
| Function&nbsp;signature | `string1 + string2...` |
| Inputs                  | Two or more strings. |
| Operations              | All input string parameters are concatenated with each other. |
| Output                  | One string. |
<br/>

| UpperCase               |   |
|---|---|
| Description             | The UpperCase function converts all characters in a string to upper case. |
| Function&nbsp;signature | `String UpperCase(string)` |
| Inputs                  | One string. |
| Operations              | All lower case characters of the input parameter are converted to upper case characters. For example: `UpperCase("test")` results in `"TEST"`. |
| Output                  | One string. |
<br/>

| LowerCase               |   |
|---|---|
| Description             | The LowerCase function converts all characters in a string to lower case. |
| Function&nbsp;signature | `String LowerCase(string)` |
| Inputs                  | One string. |
| Operations              | All upper case characters of the input parameter are converted to lower case characters. For example: `LowerCase("TeSt")` results in `"test"`. |
| Output                  | One string. |
<br/>

| ProperCase              |   |
|---|---|
| Description             | The ProperCase function converts the first character of each space-delimited word in a string to upper case. All other characters are converted to lower case. |
| Function&nbsp;signature | `String ProperCase(string)` |
| Inputs                  | One string. |
| Operations              | The first character of every space-delimited word in the input parameter is converted to upper case. All upper case characters are converted to lower case characters. If a word in the input parameter starts with a non-alphabet character, the first character of the word is not converted to upper case. For example:<ul><li>`ProperCase("TEsT")` results in `"Test"`.</li><li>`ProperCase("britta simon")` results in `"Britta Simon"`.</li><li>`ProperCase(" TEsT")` results in `" Test"`.</li><li>`ProperCase("\$TEsT")` results in `"\$Test"`.</li></ul> |
| Output                  | One string. |
<br/>

| LTrim                   |   |
|---|---|
| Description             | The LTrim function removes the leading white-spaces from a string. |
| Function&nbsp;signature | `String LTrim(string)` |
| Inputs                  | One string. |
| Operations              | The leading white-space characters contained in the input parameter are removed. For example: `LTrim(" Test ")` results in `"Test "`. |
| Output                  | One string. |
<br/>

| RTrim                   |   |
|---|---|
| Description             | The RTrim function removes the trailing white-spaces from a string. |
| Function&nbsp;signature | `String RTrim(string)` |
| Inputs                  | One string. |
| Operations              | The trailing white-space characters contained in the input parameter are removed. For example: `RTrim(" Test ")` results in `" Test"`. |
| Output                  | One string. |
<br/>

| Trim                    |   |
|---|---|
| Description             | The Trim function removes the leading and trailing white-spaces from a string. |
| Function&nbsp;signature | `String Trim(string)` |
| Inputs                  | One string. |
| Operations              | The leading and trailing white-space characters contained in the string are removed. For example: `Trim(" Test ")` results in `"Test"`. |
| Output                  | One string. |
<br/>                                                                                                                    |

| RightPad                |   |
|---|---|
| Description             | The RightPad function right-pads a string to a specified length by using a provided padding character. |
| Function&nbsp;signature | `String RightPad(string, length, padCharacter)` |
| Inputs                  | <ul><li>**string**: The string to pad.</li><li>**length**: An integer representing the desired length of the string.</li><li>**padCharacter**: A string that consists of a single character to be used as a pad character.</li></ul> |
| Operations              | If the length of **string** is less than **length**, then the **padCharacter** is repeatedly appended to the end of **string** until the **string** length is equal to **length**. For example:<ul><li>`RightPad("User", 10, "0")` results in `"User000000"`.</li><li>`RightPad(RandomNum(1,10), 5, "0")` might result in `"9000"`.</li></ul> |
| Output                  | If **string** has a length greater than or equal to **length**, a string identical to **string** is returned. If the length of **string** is less than **length**, a new string of the desired length is returned. The new string contains **string** padded with a **padCharacter**. If **string** is null, the function returns an empty string. <br/><br/>**Note**: **padCharacter** can be a space character, but it cannot be a null value. If the length of **string** is equal to or greater than **length**, **string** is returned unchanged. |
<br/>

| LeftPad                 |   |
|---|---|
| Description             | The LeftPad function left-pads a string to a specified length by using a provided padding character. |
| Function&nbsp;signature | `String LeftPad(string, length, padCharacter)` |
| Inputs                  | <ul><li>**string**: The string to pad.</li><li>**length**: An integer representing the desired length of the string.</li><li>**padCharacter**: A string that consists of a single character to be used as a pad character.</li></ul> |
| Operations              | If the length of **string** is less than **length**, then the **padCharacter** is repeatedly appended to the beginning of **string** until the **string** length is equal to **length**. For example:<ul><li>`RightPad("User", 10, "0")` results in `"000000User"`.</li><li>`RightPad(RandomNum(1,10), 5, "0")` might result in `"0009"`.</li></ul> |
| Output                  | If **string** has a length greater than or equal to **length**, a string identical to **string** is returned. If the length of **string** is less than **length**, a new string of the desired length is returned. The new string contains **string** padded with a **padCharacter**. If **string** is null, the function returns an empty string.<br/><br/>**Note**: **padCharacter** can be a space character, but it cannot be a null value. If the length of **string** is equal to or greater than **length**, **string** is returned unchanged. |
<br/>

| BitOr                   |   |
|---|---|
| Description             | The BitOr function sets a specified bit on a flag to 1. |
| Function&nbsp;signature | `Int BitOr(mask, flag)` |
| Inputs                  | <ul><li>**mask**: A hexadecimal value that specifies the bit to set on the **flag**.</li><li>**flag**: A hexadecimal value that is to have a specific bit modified.</li></ul> |
| Operations              | This function converts both parameters to binary representation and compares them:<ul><li>Sets a bit to 1 if one or both of the corresponding bits in **mask** and **flag** are 1.</i><li>Sets a bit to 0 if both of the corresponding bits are 0.</li><li>Returns 1 in all cases, except where the corresponding bits of both parameters are 0.</li><li>The resulting bit pattern is the "set" (1 or true) bits of any of the two operands.</li><li>Multiple **flag** bits can be set if multiple bits have the value 1 in **mask**.</li></ul> |
| Output                  | A new version of **flag** with the bits specified in **mask** set to 1. |
<br/>

| BitAnd                  |   |
|---|---|
| Description             | The BitAnd function sets a specified bit on a flag to 0. |
| Function&nbsp;signature | `Int BitOr(mask, flag)` |
| Inputs                  | <ul><li>**mask**: A hexadecimal value that specifies the bit to modify on the **flag**.</li><li>**flag**: A hexadecimal value that is to have a specific bit modified.</li></ul> |
| Operations              | This function converts both parameters to binary representation and compares them:<ul><li>Sets a bit to 0 if one or both of the corresponding bits in **mask** and **flag** are 0.</li><li>Sets a bit to 1 if both of the corresponding bits are 1.</li><li>Returns 0 in all cases, except where the corresponding bits of both parameters are 1.</li><li>Multiple **flag** bits can be set to 0 if multiple bits have the value 0 in **mask**. |
| Output                  | A new version of **flag** with the bits specified in **mask** set to 0. |
<br/>

| DateTimeFormat          |   |
|---|---|
| Description             | The DateTimeFormat function is used to format a DateTime in string form to a specified format. |
| Function&nbsp;signature | `String DateTimeFormat(dateTime, format)` |
| Inputs                  | <ul><li>**dateTime**: A string representing the DateTime to format.</li><li>**format**: A string representing the format to convert to.</li></ul> <br/>**Note**: For the characters that are accepted to create user-defined formats, see [User-defined Date/Time formats](http://go.microsoft.com/fwlink/?LinkId=195182). |
| Operations              | The format string specified in **format** is applied to the DateTime in the **dateTime** string. The string specified in **format** must be a valid DateTime format. If it is not, an error is returned indicating that the format is not a valid DateTime format. For example: `DateTime("12/25/2007", "yyyy-MM-dd")` results in `"2007-12-25"`. |
| Output                  | A string resulting from applying **format** to **dateTime**. |
<br/>

| ConvertSidToString      |   |
|---|---|
| Description             | The ConvertSidToString converts a byte array containing a security identifier to a string. |
| Function&nbsp;signature | `String ConvertSidToString(ObjectSID)` |
| Inputs                  | **ObjectSID**: A byte array containing a security identifier (SID). |
| Operations              | The specified binary SID is converted to a string. |
| Output                  | A string representation of the SID. |
<br/>

| ConvertStringToGuid     |   |
|---|---|
| Description             | The ConvertStringToGuid function converts the string representation of a GUID to a binary representation of the GUID. |
| Function&nbsp;signature | `Byte[] ConvertStringToGuid(stringGuid)` |
| Inputs                  | **stringGuid**: A string formatted in the pattern `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`, where the value of the GUID is represented as a series of hexadecimal digits in groups of 8, 4, 4, 4, and 12 digits and separated by hyphens. An example of a return value is `382c74c3-721d-4f34-80e557657b6cbc27`. |
| Operations              | The **stringGuid** is converted to its binary representation. If the string is not a representation of a valid GUID, the function rejects the argument with the error "The parameter for the ConvertStringToGuid function must be a string representing a valid Guid." |
| Output                  | A binary representation of the GUID. |
<br/>      

| ReplaceString           |   |
|---|---|
| Description             | The ReplaceString function replaces all occurrences of a string to another string. |
| Function&nbsp;signature | `String ReplaceString(string, OldValue, NewValue)` |
| Inputs                  | <ul><li>**string**: The string in which to replace values.</li><li>**OldValue**: The string to search for and to replace.</li><li>**NewValue**: The string to replace to.</li></ul> |
| Operations              | All occurrences of **OldValue** in the **string** are replaced with **NewValue**. The function must be able to handle the special characters **\n.** new line, **\r.** carriage return, and **\t.** tab. For example: `ReplaceString("One\n\rMicrosoft\n\r\Way","\n\r"," ")` returns `"One Microsoft Way"`. |
| Output                  | A string with all occurrences of **OldValue** in the string replaced with **NewValue.** |
<br/>

## Data retrieval functions

Data retrieval functions are used to perform operations that retrieve the desired characters from a string.

| Word               |   |
|---|---|
| Description             | The Word function returns a word contained within a string, based on parameters describing the delimiters to use and the word number to return. |
| Function&nbsp;signature | `String Word(string, number, delimiters)` |
| Inputs                  | <ul><li>**string**: The string from which to return a word.</li><li>**number**: A number identifying which word number should be returned.</li><li>**delimiters**: A string representing the delimiters that should be used to identify words.</li></ul> |
| Operations              | Each string of characters in the **string** that is separated by one of the characters in **delimiters** is identified as a word. If **string** is null, the function returns an empty string. The word that was found at the position specified in **number** is returned. If **number** < 1, the function returns an empty string. For example:<ul><li>`Word("Test;of%function;", 3, ";$&%")` returns `"function"`.</li><li>`Word("Test;;Function" , 2 , ";")` returns `""` (an empty string).</li><li>`Word("Test;of%function;", 0, ";$&%")` returns `""` (an empty string).</li></ul> |
| Output                  | A string containing the word at the position the user asked for. If **string** contains less than **number** of words, or **string** does not contain any words identified by **delimiters,** an empty string is returned. |
<br/>

| Left                    |   |
|---|---|
| Description             | The Left function returns a specified number of characters from the left (beginning) of a string. |
| Function&nbsp;signature | `String Left(string, numChars)` |
| Inputs                  | <ul><li>**string**: The string from which to return characters.</li><li>**numChars**: A number identifying the number of characters to return from the beginning of a string.</li></ul> |
| Operations              | **numChars** characters are returned from the first position of the **string**. For example: `Left("Britta Simon", 3)` returns `"Bri"`. |
| Output                  | A string containing the first **numChars** characters in the **string**. If **numChars** = 0, the function returns an empty string. If **numChars** < 0, the function returns an input string. If **string** is null, the function returns an empty string. |
<br/>

| Right                   |   |
|---|---|
| Description             | The Right function returns a specified number of characters from the right (end) of a string. |
| Function&nbsp;signature | `String Right(string, numChars)` |
| Inputs                  | <ul><li>**string**: The string from which to return characters.</li><li>**numChars**: A number identifying the number of characters to return from the end of a string.</li></ul> |
| Operations              | Return **numChars** characters from the end of the **string**. For example: `Right("Britta Simon", 3)` returns `"mon"`. |
| Output                  | A string containing the last **numChars** characters in the **string**. If **numChars** = 0, the function returns an empty string. If **numChars** < 0, the function returns an input string. If **string** is null, the function returns an empty string. If **string** contains fewer characters than the number specified in **numChars**, **string** is returned. |
<br/>

| Mid                     |   |
|---|---|
| Description             | The Mid function returns a specified number of characters from a specified position in a string. |
| Function&nbsp;signature | `String Mid(string, pos, numChars)` |
| Inputs                  | <ul><li>**string**: The string from which to return characters.</li><li>**pos**: A number identifying the starting position in a string for returning characters.</li><li>**numChars**: A number identifying the number of characters to return from a position in the string.</li></ul> |
| Operations              | Return **numChars** characters starting from position **pos** in the **string**. For example: `Mid("Britta Simon", 3, 5)` returns `"itta "`. |
| Output                  | A string containing **numChars** characters from position **pos** in the **string**. If **numChars** = 0, the function returns an empty string. If **numChars** < 0, the function returns an empty string. If **pos** > the length of **string**, the function returns an input string. If **pos** <= 0, the function returns an input string. If **string** is null, the function returns an empty string. If there are no **numChar** characters remaining in **string** from position **pos**, as many characters as possible are returned. |
<br/>


## Data generation functions

Data generation functions are used to generate values for specific data types.

| CRLF                    |   |
|---|---|
| Description             | The CRLF function generates a Carriage Return/Line Feed. Use this function to add a new line. |
| Function&nbsp;signature | `String CRLF` |
| Inputs                  | None. |
| Operations              | A CRLF is returned. For example:<br/>`AddressLine1 + CRLF() + AddressLine2`<br/>results in `AddressLine1`<br/>`AddressLine2`. |
| Output                  | A CRLF is the output. |
<br/>

| RandomNum               |   |
|---|---|
| Description             | The RandomNum function returns a random number within a specified interval. |
| Function&nbsp;signature | `Int RandomNum(start, end)` |
| Inputs                  | <ul><li>**start**: A number identifying the lower limit of the random value to generate.</li><li>**end**: A number identifying the upper limit of the random value to generate.</li></ul> |
| Operations              | A random number greater than or equal to **start** and less than or equal to **end** is generated. For example: `Random(0,999)` might return `100`. |
| Output                  | A random number within the range specified by **start** and **end**. |
<br/>

| EscapeDNComponent       |   |
|---|---|
| Description             | The EscapeDNComponent method from MIISUtils is used to perform this operation. This method processes the input string based on the type of management agent (MA) that's being used. |
| Function&nbsp;signature | `String EscapeDNComponent(string)` |
| Inputs                  | **string**: A string that's used to process a distinguished name. The string should not contain escaped characters. |
| Operations              | Different MAs require different distinguished name formats. This method processes the input **string** based on the following MA types:<ul><li>**LDAP distinguished name**: Examples of this MA type include Active Directory Domain Services, Sun Directory Server (formerly iPlanet Directory Server), and Microsoft Exchange Server.<ul><li>Any invalid XML characters in the value portion of a given part are hexadecimal-encoded.</li><li>Any illegal characters (including invalid XML characters) in the name portion of a given part generate an error.</li><li>Escaped characters include comma (,), equals (=), plus (+), less than (<), greater than (>), number (#), semi-colon (;), backslash (\), and double quote (").</li><li>If the last character in the **string** is a space ( ), that space is escaped.</li><li>Any extraneous leading or trailing spaces around a part name are removed.</li><li>For the XML MA, if there are multiple parts, then the parts are alphabetized.</li><li>If multiple parts are specified, the composite distinguished name string is the concatenation of the individual strings separated by plus (+) signs.</li><li>An error is generated if the input **string** is not a well-formed, LDAP-style distinguished name string.</li></ul></li><li>**Hierarchical non-LDAP**: An example of this type of MA is Microsoft Lotus Notes.<ul><li>This MA type doesn't support multipart components.</li><li>If multiple strings are passed to `EscapeDNComponent`, an **ArgumentException** is thrown.</li><li>If any of the characters in the input **string** are invalid XML characters, an **ArgumentException** is thrown.</li><li>All commas (,) and backslashes (/) in the input **string** are escaped.</li><li>If the last character in the **string** is a space ( ), that space is escaped.</li></ul></li><li>**Extrinsic**: Examples of this MA type include databases or XML without an LDAP distinguished name.<ul><li>If any part is binary or contains an invalid XML character, that part is stored as a hexadecimal-encoded version of the raw data with a number (#) character prefixed to the front of the string. For example, if a part was `AxC` (where x represents an illegal XML character such as `0x10`), that part is encoded as `#410010004300`. Otherwise, all instances of these characters are escaped: backslash (\), comma (,), plus (+), and number (#).</li><li>If the last character in a given part string is a space ( ), that space is escaped.</li><li>If multiple parts are specified, the composite distinguished name string is the concatenation of all the individual strings separated by plus signs.</li></ul></li> |
| Output                  | A string containing a valid domain name. |

>[!NOTE]
>
>The validation of distinguished names is less strict than the syntax defined in the LDAP specifications. `EscapeDNComponent(String[])` allows a part name to contain any combination of one or more of the characters 'a'-'z', 'A'-'Z', '0'-'9', '-', and '.'.
>
>It's not possible to specify a binary part with this method. However, it is possible to have a binary part in **CommitNewConnector** if the distinguished name is constructed from anchor attributes and one of the anchor attributes is a binary type.
>
<br/>


| Null                    |   |
|---|---|
| Description             | The Null function is used to specify that the MA doesn't have an attribute to contribute, and that attribute precedence should continue with the next MA. |
| Function&nbsp;signature | `String Null` |
| Inputs                  | None. |
| Operations              | A Null is returned. For example: `IIF(Eq(domain), "unknown", Null())` returns `Null`. |
| Output                  | A Null is output. |
<br/>


## Logic Functions
Logic functions are used to perform an operation based on conditions evaluated by the system.


| IIF                     |   |
|---|---|
| Description             | The IIF function returns one of a set of possible values based on a specified condition. |
| Function&nbsp;signature | `Object IIF(condition, valueIfTrue, valueIfFalse)` |
| Inputs                  | <ul><li>**condition**: Any value or expression that can be evaluated to true or false. The following functions are available for use as expressions in the IIF function for the **condition:**<ul><li>**Eq**: This function compares two arguments for equality.</li><li>**NotEquals**: This function compares two arguments for inequality, returning true if they are not equal and false if they are equal. For example: `NotEquals(EmployeeType, "Contractor")`.</li><li>**LessThan**: This function compares two numbers, returning true if the first is less than the second and false otherwise. For example: `LessThan(Salary, 100000)`.</li><li>**GreaterThan**: This function compares two numbers, returning true if the first is greater than the second and false otherwise. For example: `GreaterThan(Salary, 100000)`.</li><li>**LessThanOrEquals**: This function compares two numbers, returning true if the first is less than or equal to the second and false otherwise. For example: `LessThanOrEquals(Salary, 100000)`.</li><li>**GreaterThanOrEquals**: This function compares two numbers, returning true if the first is greater than or is equal to the second and false otherwise. For example: `GreaterThanOrEquals(Salary, 100000)`.</li><li>**IsPresent**: This function takes as input an attribute in the ILM schema and returns true if the attribute is not null and false if the attribute is null.</li></ul></li><li>**valueIfTrue**: A value that is returned if the **condition** evaluates to true.</li><li>**valueIfFalse**: A value that is returned if the **condition** evaluates to false.</li></ul> |
| Operations              | If **condition** evaluates to true, the function returns **valueIfTrue**. Otherwise, the function returns **valueIfFalse**. For example: `IIF(Eq(EmployeeType,"Intern"),"t-" + Alias, Alias)` returns the alias of a user with "t-" added to the beginning of the alias if the user is an intern. Otherwise, the function returns the user’s alias as-is. |
| Output                  | The output is **valueIfTrue** if the condition is true, or **valueIfFalse** if the condition is false. |
<br/>
