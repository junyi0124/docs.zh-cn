---
title: <xmlSerializer> 元素
description: <xmlSerializer> 元素指定是否完成 XmlSerializer 进度的额外检查。
ms.date: 03/30/2017
helpviewer_keywords:
- <xmlSerializer> element
- XML serialization, configuration
- xmlSerializer element
ms.assetid: d129d10c-3eb7-45d9-8098-5fa853825e47
ms.openlocfilehash: 6f368880c97a21dc3e9593ecb2319d265a1b8932
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95676486"
---
# <a name="xmlserializer-element"></a>\<xmlSerializer> 元素

指定是否完成 <xref:System.Xml.Serialization.XmlSerializer> 进度的额外检查。  
  
 \<configuration>  
\<system.xml.serialization>  
  
## <a name="syntax"></a>语法  
  
```xml  
<xmlSerializer checkDeserializerAdvance = "true|false" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|特性|描述|  
|---------------|-----------------|  
|**checkDeserializeAdvances**|指定是否已检查 <xref:System.Xml.Serialization.XmlSerializer> 的进度。 将特性设置为“true”或“false”。 默认值为“true”。|  
|**useLegacySerializationGeneration**|指定 <xref:System.Xml.Serialization.XmlSerializer> 是否使用旧的序列化生成，该方法通过将 C# 代码写入到一个文件，然后将其编译为程序集来生成程序集。 默认值为 false。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<system.xml.serialization> 元素](system-xml-serialization-element.md)|包含 <xref:System.Xml.Serialization.XmlSerializer> 和 <xref:System.Xml.Serialization.XmlSchemaImporter> 类的配置设置。|  
  
## <a name="remarks"></a>备注  

 默认情况下，当反序列化不受信任的数据时，<xref:System.Xml.Serialization.XmlSerializer> 会额外提供一层防范潜在拒绝服务攻击的安全保护。 它通过在反序列化期间尝试检测无限循环来实现以上保护。 若检测到此类情况，将引发异常，并出现以下消息：“内部错误: 反序列化无法越过基础流。”  
  
 接收到此消息并不一定表示正在发生拒绝服务攻击。 在某些极少出现的情况下，无限循环检测机制会产生误报，并对合法的传入消息引发异常。 如果发现在你的特定应用程序中，合法消息被这一额外的保护层拒绝，请将 checkDeserializeAdvances 属性设置为“false”。  
  
## <a name="example"></a>示例  

 下面的代码示例将 checkDeserializeAdvances 属性设置为“false”。  
  
```xml  
<configuration>  
  <system.xml.serialization>  
    <xmlSerializer checkDeserializeAdvances="false" />  
  </system.xml.serialization>  
</configuration>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.Serialization.XmlSerializer>
- [\<system.xml.serialization> 元素](system-xml-serialization-element.md)
- [XML 和 SOAP 序列化](xml-and-soap-serialization.md)
