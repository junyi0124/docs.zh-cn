---
title: <schemaImporterExtensions> 的 <add> 元素
description: <add> 元素可添加 XmlSchemaImporter 在将 XSD 类型映射到 .NET 类型时所用的类型。
ms.date: 03/30/2017
helpviewer_keywords:
- XML serialization, configuration
- <add> element for <schemaImporterExtensions> element
ms.assetid: c828a558-094b-441e-9065-790b87315fa0
ms.openlocfilehash: b8a0775e9d33d59606b1150aa9a1b3b1026d4b0b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726439"
---
# <a name="add-element-for-schemaimporterextensions"></a>\<schemaImporterExtensions> 的 \<add> 元素

添加将 XSD 类型映射到 .NET 类型时 <xref:System.Xml.Serialization.XmlSchemaImporter> 所用的类型。 有关配置文件的详细信息，请参阅[配置文件架构](../../framework/configure-apps/file-schema/index.md)。  
  
\<configuration>  
\<system.xml.serialization>  
\<schemaImporterExtensions>  
\<add>  
  
## <a name="syntax"></a>语法  
  
```xml  
<add name = "typeName" type="fully qualified type [,Version=version number] [,Culture=culture] [,PublicKeyToken= token]"/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|特性|描述|  
|---------------|-----------------|  
|**name**|用于查找实例的简单名称。|  
|**type**|必需。 指定要添加的架构扩展类。 type 特性值必须位于一行上，并且包含完全限定的类型名称。 当程序集放置在全局程序集缓存 (GAC) 中时，该特性值还必须包括已签名程序集的版本、区域性和公钥标记。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|\<schemaImporterExtensions>|包含 <xref:System.Xml.Serialization.XmlSchemaImporter> 所使用的类型。|  
  
## <a name="example"></a>示例  

 下面的代码示例添加 XmlSchemaImporter 可以在映射类型时使用的扩展类型。  
  
```xml  
<configuration>  
  <system.xml.serialization>  
    <schemaImporterExtensions>  
       <add name="contoso" type="System.Web.Mobile.MobileCapabilities,
       System.Web.Mobile, Version=2.0.0.0, Culture=neutral,
       PublicKeyToken=b03f5f7f11d50a3a" />
    </schemaImporterExtensions>  
  </system.xml.serialization>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.Serialization.XmlSchemaImporter>
- [\<system.xml.serialization> 元素](system-xml-serialization-element.md)
- [\<schemaImporterExtensions> 元素](schemaimporterextensions-element.md)
