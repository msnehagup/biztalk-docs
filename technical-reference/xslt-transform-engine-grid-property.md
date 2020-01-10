---
# required metadata

title: XSLT Transform Engine (Grid Property)
description: XSLT Transform Engine (Grid Property).
author: Elvis-Shi
ms.author: elsh
manager: dougeby
ms.date: 01/06/2029
ms.topic: conceptual
ms.prod: biztalk-server
# optional metadata

#ROBOTS:

ms.reviewer: 
ms.suite:
ms.tgt_pltfrm:
ms.assetid: 
ms.custom: biztalk-2020
---

# XSLT Transform Engine (Grid Property)

**Applicable starting BizTalk Server 2020**, user can choose Saxon:registered: as XSLT transform engine. It is also possible to plug-in your own XSLT transform engine. Use **XSLT Transform Engine** property to specify the XSLT transform engine you wish to use.

BizTalk's default XSL transformation is implemented on top of .Net Framework [XSLT Transformations](https://docs.microsoft.com/en-us/dotnet/standard/data/xml/xslt-transformations). This support is limited to XSLT 1.0. Use this property to configure other XSL transform engines at map level. This makes it possible for BizTalk server maps to support newer versions of XSLT. Using Sexon:registered: one can readily use XSLT3.0.

## Category

Compiler

## Allowed Values

<table>
<thead>
<tr class="header">
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Undefined</td>
<td>Use global XSLT transform engine setting. No map specific override is applied.</td>
</tr>
<tr class="even">
<td>.Net Framework</td>
<td>Use ".Net Framework" XSLT transform engine for this map. Property "Use XSL Transform" will be applied in this case.</td>
</tr>
<tr class="odd">
<td>Saxon 9 HE</td>
<td>Use "Saxon-HE 9" XSLT transform engine. Visit www.saxonica.com for more information. </td>
</tr>
<tr class="odd">
<td>Other custom XSLT transform</td>
<td>Use custom XSLT transform engine. More information on how to implement and use custom XSLT transform engine follows. </td>
</tr>
</tbody>
</table>


## Default Value

Undefined

## Create custom XSLT transform

Users can plug-in any XSL transform engine by implementing abstract class Microsoft.XLANGs.BaseTypes.ITransform2. DLL implementing this capability must be present at "\Program Files (x86)\Microsoft BizTalk Server\Transform Components\" on every BizTalk runtime machine. 

- [Custom XSLT transform implementation](xslt-custom-transform-implementation.md)

To use this custom transformer in Visual Studio during design time, update "\Program Files (x86)\Microsoft BizTalk Server\Developer Tools\CustomTransform.xml".
- Add a new "Transform" node 
- Add sub node "DisplayName" with text to be displayed in the drop-down list for "XSLT transform engine" property
- Add sub node "TypeAssemblyQualifiedName" with details of class that implements your custom transform engine

Below is an example how Saxon-HE 9 reference implementation is done:

```xml
	<Transform 
		DisplayName="Saxon 9 HE"
		TypeAssemblyQualifiedName="Microsoft.XLANGs.BaseTypes.SaxonHEXsltTransform, Microsoft.XLANGs.BaseTypes, Version=3.0.1.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
 ```

Your custom transformer will show up in "XSLT transform engine" drop-down list after Visual Studio restart.

## Global XSLT transform engine

BizTalk’s default XSLT transform engine setting is present in BizTalk Server registry "XsltEngine" at HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\BizTalk Server\3.0\Configuration.  This can be used to override the default value of “.Net Framework”. To override this value, use AssemblyQualifiedName of the class implementing the transformation engine.

## Saxon:registered: 9 transform engine
>[!IMPORTANT]
>Saxon 9 doesn't support embedded scripting . As a result, functoids shipped as part of BizTalk may not function well with Saxon 9. 

You must refer to Saxon:registered: documentation for scope of XSLT and Xpath support. If you wish to use other editions, create custom XSLT transform based on these editions. 

**Custom Extension XML** is still a supported way for creating your custom extension for Saxon 9 HE transform engine. Create custom .Net extension functions by implementing interface `ExtensionFunction` or `ExtensionFunctionDefinition`, and add your implementations into **Custom Extension XML**.  Saxon 9 HE transform engine will register extension functions defined in **Custom Extension XML**, and transform processor can then recognize and invoke any call from XSLT.



## Remark

> [!NOTE]
> <P>You cannot undo or redo the <STRONG>XSLT transform engine</STRONG> property.</P>



## See Also

[Grid Properties](grid-properties.md)

[Custom Extension XML](custom-extension-xml-grid-property.md)

