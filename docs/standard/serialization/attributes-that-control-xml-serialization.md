---
title: 用来控制 XML 序列化的属性
description: 本文包含可应用于类和类成员的属性，它们用于控制 XmlSerializer 序列化或反序列化该类的实例的方式。
ms.date: 03/30/2017
helpviewer_keywords:
- classes, serializing
- XmlSerializer class, serializing
- XML serialization, attributes
- attributes [.NET], XML serialization
- serialization, attributes
- XML Schema, serializing
ms.assetid: 414b820f-a696-4206-b576-2711d85490c7
ms.openlocfilehash: 55c36ea6313bfd3000b1d8502d4eda7c552e08cf
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95717924"
---
# <a name="attributes-that-control-xml-serialization"></a>用来控制 XML 序列化的属性

通过将下表中的特性应用于类和类成员，可以控制 <xref:System.Xml.Serialization.XmlSerializer> 序列化或反序列化该类的实例的方式。 若要了解这些属性如何控制 XML 序列化，请参阅[使用属性控制 XML 序列化](controlling-xml-serialization-using-attributes.md)。  
  
 这些属性还可用于控制 XML Web services 生成的文本样式的 SOAP 消息。 有关将这些属性应用于 XML Web service 方法的更多信息，请参阅 [使用 XML Web service 进行 XML 序列化](xml-serialization-with-xml-web-services.md)。  
  
 有关属性的详细信息，请参阅[属性](../attributes/index.md)。  
  
|特性|适用对象|指定|  
|---------------|----------------|---------------|  
|<xref:System.Xml.Serialization.XmlAnyAttributeAttribute>|公共字段、属性、参数或返回 <xref:System.Xml.XmlAttribute> 对象数组的返回值。|反序列化时，将会使用 <xref:System.Xml.XmlAttribute> 对象填充数组，而这些对象代表对于架构未知的所有 XML 特性。|  
|<xref:System.Xml.Serialization.XmlAnyElementAttribute>|公共字段、属性、参数或返回 <xref:System.Xml.XmlElement> 对象数组的返回值。|反序列化时，将会使用 <xref:System.Xml.XmlElement> 对象填充数组，而这些对象代表对于架构未知的所有 XML 元素。|  
|<xref:System.Xml.Serialization.XmlArrayAttribute>|公共字段、属性、参数或返回复杂对象的数组的返回值。|数组成员将作为 XML 数组的成员生成。|  
|<xref:System.Xml.Serialization.XmlArrayItemAttribute>|公共字段、属性、参数或返回复杂对象的数组的返回值。|可以插入数组的派生类型。 通常与 <xref:System.Xml.Serialization.XmlArrayAttribute> 一起应用。|  
|<xref:System.Xml.Serialization.XmlAttributeAttribute>|公共字段、属性、参数或返回值。|成员将作为 XML 属性进行序列化。|  
|<xref:System.Xml.Serialization.XmlChoiceIdentifierAttribute>|公共字段、属性、参数或返回值。|可以使用枚举进一步消除成员的歧义。|  
|<xref:System.Xml.Serialization.XmlElementAttribute>|公共字段、属性、参数或返回值。|字段或属性将作为 XML 元素进行序列化。|  
|<xref:System.Xml.Serialization.XmlEnumAttribute>|作为枚举标识符的公共字段。|枚举成员的元素名称。|  
|<xref:System.Xml.Serialization.XmlIgnoreAttribute>|公共属性和公共字段。|序列化包含类时，应该忽略属性或字段。|  
|<xref:System.Xml.Serialization.XmlIncludeAttribute>|公共派生类声明，以及 Web 服务描述语言 (WSDL) 文档的公共方法的返回值。|生成要在序列化时识别的架构时，应该将该类包括在内。|  
|<xref:System.Xml.Serialization.XmlRootAttribute>|公共类声明。|控制视为 XML 根元素的属性目标的 XML 序列化。 使用该属性可进一步指定命名空间和元素名称。|  
|<xref:System.Xml.Serialization.XmlTextAttribute>|公共属性和公共字段。|属性或字段应该作为 XML 文本进行序列化。|  
|<xref:System.Xml.Serialization.XmlTypeAttribute>|公共类声明。|XML 类型的名称和命名空间。|  
  
 除了这些特性（全部位于 <xref:System.Xml.Serialization> 命名空间中）之外，还可以将 <xref:System.ComponentModel.DefaultValueAttribute> 特性应用于字段。 如果没有指定值，使用 DefaultValueAttribute 可设置将自动分配给成员的值。  
  
 若要控制编码的 SOAP XML 序列化，请参阅[控制编码的 SOAP 序列化的特性](attributes-that-control-encoded-soap-serialization.md)。  
  
## <a name="see-also"></a>请参阅

- [XML 和 SOAP 序列化](xml-and-soap-serialization.md)
- <xref:System.Xml.Serialization.XmlSerializer>
- [使用属性控制 XML 序列化](controlling-xml-serialization-using-attributes.md)
- [如何：指定 XML 流的替代元素名称](how-to-specify-an-alternate-element-name-for-an-xml-stream.md)
- [如何：序列化对象](how-to-serialize-an-object.md)
- [如何：反序列化对象](how-to-deserialize-an-object.md)
