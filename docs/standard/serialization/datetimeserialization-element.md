---
title: <dateTimeSerialization> 元素
description: 本文介绍 <dateTimeSerialization> 元素，该元素可确定 DateTime 对象的序列化模式。
ms.date: 03/30/2017
helpviewer_keywords:
- dateTimeSerialization element
- XML serialization, configuration
- <dateTimeSerialization> element
ms.assetid: 90fda55c-7730-41e9-bc4b-6423a4b920af
ms.openlocfilehash: 1623517e66955c14b7e738c860ec16086fe30429
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95678969"
---
# <a name="datetimeserialization-element"></a>\<dateTimeSerialization> 元素

确定 <xref:System.DateTime> 对象的序列化模式。  
  
 \<configuration>  
\<dateTimeSerialization>  
  
## <a name="syntax"></a>语法  
  
```xml  
<dateTimeSerialization  
    mode = "Roundtrip|Local"  
/>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|特性|描述|  
|----------------|-----------------|  
|`mode`|可选。 指定序列化模式。 设置为 <xref:System.Xml.Serialization.Configuration.DateTimeSerializationSection.DateTimeSerializationMode> 值之一。 默认值为 RoundTrip。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|system.xml.serialization|用于控制 XML 序列化的顶级元素。|  
  
## <a name="remarks"></a>备注  

将此属性设置为 Local 时，<xref:System.DateTime> 对象始终设置为本地时间格式。 即，序列化的数据中总是包含本地时区信息。
  
将此属性设置为 Roundtrip 时，系统会检查 <xref:System.DateTime> 对象以确定这些对象位于本地时区、UTC 时区还是未指定的时区中。 随后会序列化 <xref:System.DateTime> 对象并保留该信息。 这是默认行为，建议为所有不与 Framework 的较早版本通信的新应用程序使用此行为。  
  
## <a name="see-also"></a>请参阅

- <xref:System.DateTime>
- <xref:System.Xml.Serialization.XmlSchemaImporter>
- <xref:System.Xml.Serialization.Configuration.DateTimeSerializationSection.DateTimeSerializationMode>
- [配置文件架构](../../framework/configure-apps/file-schema/index.md)
- [\<schemaImporterExtensions> 元素](schemaimporterextensions-element.md)
- [\<add> 的 \<schemaImporterExtensions>](add-element-for-schemaimporterextensions.md) 元素
- [\<system.xml.serialization> 元素](system-xml-serialization-element.md)
