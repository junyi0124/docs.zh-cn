---
title: <system.xml.serialization> 元素
description: 本文介绍 <system.xml.serialization> 元素，该元素是用于控制 XML 序列化的顶级元素。
ms.date: 03/30/2017
helpviewer_keywords:
- system.xml.serialization element
- XML serialization, configuration
- <system.xml.serialization> element
ms.assetid: 3ce45919-388a-418c-8968-6df0372c73ec
ms.openlocfilehash: 6291799aadc429e943996f2256d773ac36dd370f
ms.sourcegitcommit: 74d05613d6c57106f83f82ce8ee71176874ea3f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93282389"
---
# <a name="systemxmlserialization-element"></a>\<system.xml.serialization> 元素

用于控制 XML 序列化的顶级元素。 有关配置文件的详细信息，请参阅[配置文件架构](../../framework/configure-apps/file-schema/index.md)。

\<configuration>\
\<system.xml.serialization>

## <a name="syntax"></a>语法

```xml
<system.xml.serialization>
</system.xml.serialization>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

无。

### <a name="child-elements"></a>子元素

|元素|描述|
|-------------|-----------------|
|[\<dateTimeSerialization> 元素](datetimeserialization-element.md)|确定 <xref:System.DateTime> 对象的序列化模式。|
|[\<schemaImporterExtensions> 元素](schemaimporterextensions-element.md)|包含将 XSD 类型映射到 .NET 类型时 <xref:System.Xml.Serialization.XmlSchemaImporter> 所用的类型。|

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|[\<configuration> 元素](../../framework/configure-apps/file-schema/configuration-element.md)|公共语言运行库和 .NET Framework 应用程序所使用的每个配置文件中的根元素。|

## <a name="example"></a>示例

下面的代码示例演示如何指定 <xref:System.DateTime> 对象的序列化模式，以及将 XSD 类型映射到 .NET 类型时 <xref:System.Xml.Serialization.XmlSchemaImporter> 所用的其他类型。

```xml
<system.xml.serialization>
    <xmlSerializer checkDeserializeAdvances="false" />
    <dateTimeSerialization mode = "Local" />
    <schemaImporterExtensions>
        <add
        name = "MobileCapabilities"
        type = "System.Web.Mobile.MobileCapabilities,
        System.Web.Mobile, Version=2.0.0.0, Culture=neutral,
        PublicKeyToken=b03f5f6f11d40a3a" />
    </schemaImporterExtensions>
</system.xml.serialization>
```

## <a name="see-also"></a>请参阅

- <xref:System.Xml.Serialization.XmlSchemaImporter>
- <xref:System.Xml.Serialization.Configuration.DateTimeSerializationSection.DateTimeSerializationMode>
- [配置文件架构](../../framework/configure-apps/file-schema/index.md)
- [\<dateTimeSerialization> 元素](datetimeserialization-element.md)
- [\<schemaImporterExtensions> 元素](schemaimporterextensions-element.md)
- [\<add> 的 \<schemaImporterExtensions>](add-element-for-schemaimporterextensions.md) 元素
