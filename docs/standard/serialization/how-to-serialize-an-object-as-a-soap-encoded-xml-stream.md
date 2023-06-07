---
title: 如何：将对象序列化为 SOAP 编码的 XML 流
description: 了解如何将对象序列化为 SOAP 编码的 XML 流。 XmlSerializer 类可用于序列化类和生成编码的 SOAP 消息。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- SOAP, XML serialization
- XML serialization, SOAP
- serialization, SOAP
ms.assetid: af406e0a-fa3a-46dd-a7ba-c80731eba3a0
ms.openlocfilehash: 1d38c4e334439ef41b4d4429e52cff04c6463573
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "84291560"
---
# <a name="how-to-serialize-an-object-as-a-soap-encoded-xml-stream"></a>如何：将对象序列化为 SOAP 编码的 XML 流
  
 由于 SOAP 消息是使用 XML 生成的，因此 <xref:System.Xml.Serialization.XmlSerializer> 类可用于序列化类和生成编码的 SOAP 消息。 生成的 XML 符合[万维网联合会文档“简单对象访问协议 (SOAP) 1.1”的第 5 节](https://www.w3.org/TR/2000/NOTE-SOAP-20000508/#_Toc478383512)。 如果您要创建通过 SOAP 消息进行通信的 XML Web services，则可以将一组特殊的 SOAP 属性应用于类和类的成员来自定义 XML 流。 有关属性列表，请参阅[控制编码的 SOAP 序列化的特性](attributes-that-control-encoded-soap-serialization.md)。  
  
### <a name="to-serialize-an-object-as-a-soap-encoded-xml-stream"></a>将对象序列化为 SOAP 编码的 XML 流  
  
1. 使用 [XML 架构定义工具 (Xsd.exe)](xml-schema-definition-tool-xsd-exe.md) 创建类。  
  
2. 应用在 `System.Xml.Serialization` 中找到的一个或多个特殊属性。 请参见“用来控制编码的 SOAP 序列化的属性”中的列表。  
  
3. 通过创建新的 `XmlTypeMapping`，然后用已序列化类的类型调用 `SoapReflectionImporter` 方法，来创建 `ImportTypeMapping`。  
  
     以下代码示例调用 `SoapReflectionImporter` 类的 `ImportTypeMapping` 方法来创建 `XmlTypeMapping`。  
  
    ```vb  
    ' Serializes a class named Group as a SOAP message.  
    Dim myTypeMapping As XmlTypeMapping =
        New SoapReflectionImporter().ImportTypeMapping(GetType(Group))  
    ```  
  
    ```csharp  
    // Serializes a class named Group as a SOAP message.  
    XmlTypeMapping myTypeMapping =
        new SoapReflectionImporter().ImportTypeMapping(typeof(Group));
    ```  
  
4. 通过将 `XmlSerializer` 传递给 `XmlTypeMapping` 构造函数，来创建 <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Xml.Serialization.XmlTypeMapping%29> 类的实例。  
  
    ```vb  
    Dim mySerializer As XmlSerializer = New XmlSerializer(myTypeMapping)  
    ```  
  
    ```csharp  
    XmlSerializer mySerializer = new XmlSerializer(myTypeMapping);  
    ```  
  
5. 调用 `Serialize` 或 `Deserialize` 方法。  
  
## <a name="example"></a>示例  
  
```vb  
' Serializes a class named Group as a SOAP message.  
Dim myTypeMapping As XmlTypeMapping =
    New SoapReflectionImporter().ImportTypeMapping(GetType(Group))
Dim mySerializer As XmlSerializer = New XmlSerializer(myTypeMapping)  
```  
  
```csharp  
// Serializes a class named Group as a SOAP message.  
XmlTypeMapping myTypeMapping =
    new SoapReflectionImporter().ImportTypeMapping(typeof(Group));
XmlSerializer mySerializer = new XmlSerializer(myTypeMapping);  
```  
  
## <a name="see-also"></a>请参阅

- [XML 和 SOAP 序列化](xml-and-soap-serialization.md)
- [用来控制编码的 SOAP 序列化的属性](attributes-that-control-encoded-soap-serialization.md)
- [使用 XML Web services 进行 XML 序列化](xml-serialization-with-xml-web-services.md)
- [如何：序列化对象](how-to-serialize-an-object.md)
- [如何：反序列化对象](how-to-deserialize-an-object.md)
- [如何：替代编码的 SOAP XML 序列化](how-to-override-encoded-soap-xml-serialization.md)
