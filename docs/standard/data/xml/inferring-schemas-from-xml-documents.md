---
title: 从 XML 文档推断架构
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
ms.assetid: f3d97d53-614d-4a04-a174-87965b7405f6
ms.openlocfilehash: 4540e1706cbd3dad9490f100d7e8fa58e80a9206
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733433"
---
# <a name="inferring-schemas-from-xml-documents"></a>从 XML 文档推断架构

此主题描述如何使用 <xref:System.Xml.Schema.XmlSchemaInference> 类从 XML 文档的结构推断 XML 架构定义语言 (XSD) 架构。  
  
## <a name="the-schema-inference-process"></a>架构推断过程  

 <xref:System.Xml.Schema.XmlSchemaInference> 命名空间的 <xref:System.Xml.Schema?displayProperty=nameWithType> 类用于从 XML 文档的结构生成一个或多个 XML 架构定义语言 (XSD) 架构。 生成的架构可以用于验证原始 XML 文档。  
  
 XML 文档由 <xref:System.Xml.Schema.XmlSchemaInference> 类处理时，<xref:System.Xml.Schema.XmlSchemaInference> 类假定架构组件描述了 XML 文档中的元素和属性。 <xref:System.Xml.Schema.XmlSchemaInference> 类还通过为特定元素或属性推断限制性最强的类型，以约束的方式推断架构组件。 随着收集到的 XML 文档信息越来越多，将推断限制性较弱的类型，从而放松这些约束。 可以推断的限制性最弱的类型为 `xs:string`。  
  
 以 XML 文档的以下片断为例。  
  
```xml  
<parent attribute1="6">  
    <child>One</child>  
    <child>Two</child>  
</parent>  
<parent attribute1="A" />
```  
  
 在上述示例中，当 `attribute1` 过程遇到值为 `6` 的 <xref:System.Xml.Schema.XmlSchemaInference> 属性时，假定类型为 `xs:unsignedByte`。 当 `parent` 过程遇到第二个 <xref:System.Xml.Schema.XmlSchemaInference> 元素时，类型将修改为 `xs:string`，从而放松约束，因为现在 `attribute1` 属性的值为 `A`。 同样，在架构中推断的所有 `minOccurs` 元素的 `child` 属性均放松为 `minOccurs="0"`，因为第二个父元素没有子元素。  
  
## <a name="inferring-schemas-from-xml-documents"></a>从 XML 文档推断架构  

 <xref:System.Xml.Schema.XmlSchemaInference> 类使用两个重载 <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> 方法从 XML 文档推断架构。  
  
 第一种 <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A?displayProperty=nameWithType> 方法用于基于 XML 文档创建架构。 第二种 <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A?displayProperty=nameWithType> 方法用于推断描述多个 XML 文档的架构。 例如，可以将多个 XML 文档传入 <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A?displayProperty=nameWithType> 方法，一次传入一个，以生成描述整个 XML 文档集的架构。  
  
 第一种 <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A?displayProperty=nameWithType> 方法从 <xref:System.Xml.XmlReader> 对象中包含的 XML 文档推断架构，并返回包含推断出的架构的 <xref:System.Xml.Schema.XmlSchemaSet> 对象。 第二种 <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A?displayProperty=nameWithType> 方法在 <xref:System.Xml.Schema.XmlSchemaSet> 对象中搜索目标命名空间与 <xref:System.Xml.XmlReader> 对象中包含的 XML 文档相同的架构，精选现有架构，并返回包含推断出的架构的 <xref:System.Xml.Schema.XmlSchemaSet> 对象。  
  
 对精选的架构所做的更改基于 XML 文档中的新结构。 例如，在遍历 XML 文档时，假定发现的数据类型，并基于这些假定创建架构。 但是，如果在另一次推断过程中遇到与原始假定不同的数据，将精选该架构。 下面的示例阐释此精选过程。  
  
 [!code-cpp[XmlSchemaInferenceExamples#4](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaInferenceExamples/CPP/XmlSchemaInferenceExamples.cpp#4)]
 [!code-csharp[XmlSchemaInferenceExamples#4](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaInferenceExamples/CS/XmlSchemaInferenceExamples.cs#4)]
 [!code-vb[XmlSchemaInferenceExamples#4](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaInferenceExamples/VB/XmlSchemaInferenceExamples.vb#4)]  
  
 该示例使用以下 `item1.xml` 文件作为第一个输入。  
  
 [!code-xml[XmlSchemaInferenceExamples#13](../../../../samples/snippets/xml/VS_Snippets_Data/XmlSchemaInferenceExamples/XML/item1.xml#13)]  
  
 然后使用 `item2.xml` 文件作为第二个输入：  
  
 [!code-xml[XmlSchemaInferenceExamples#14](../../../../samples/snippets/xml/VS_Snippets_Data/XmlSchemaInferenceExamples/XML/item2.xml#14)]  
  
 在第一个 XML 文档中遇到 `productID` 属性时，`123456789` 的值将假定为 `xs:unsignedInt` 类型。 但是，在读取第二个 XML 文档并发现 `A53-246` 值时，就不再假定 `xs:unsignedInt` 类型。 将精选该架构，`productID` 的类型更改为 `xs:string`。 此外，`minOccurs` 元素的 `supplierID` 属性设置为 `0`，因为第二个 XML 文档中没有 `supplierID` 元素。  
  
 以下是从第一个 XML 文档推断的架构。  
  
 [!code-xml[XmlSchemaInferenceExamples#15](../../../../samples/snippets/xml/VS_Snippets_Data/XmlSchemaInferenceExamples/XML/InferSchema1.xml#15)]  
  
 以下是从第一个 XML 文档推断的架构，通过第二个 XML 文档进行精选。  
  
 [!code-xml[XmlSchemaInferenceExamples#16](../../../../samples/snippets/xml/VS_Snippets_Data/XmlSchemaInferenceExamples/XML/InferSchema2.xml#16)]  
  
## <a name="inline-schemas"></a>内联架构  

 如果在 <xref:System.Xml.Schema.XmlSchemaInference> 过程中遇到内联 XML 架构定义语言 (XSD) 架构，将引发 <xref:System.Xml.Schema.XmlSchemaInferenceException>。 例如，以下内联架构将引发 <xref:System.Xml.Schema.XmlSchemaInferenceException>。  
  
```xml  
<root xmlns:ex="http://www.contoso.com" xmlns="http://www.tempuri.org">  
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" targetNamespace="http://www.contoso.com">  
        <xs:element name="Contoso" type="xs:normalizedString" />  
    </xs:schema>  
    <ex:Contoso>Test</ex:Contoso>  
</root>  
```  
  
## <a name="schemas-that-cannot-be-refined"></a>无法精选的架构  

 如果给定要精选的类型，有些 W3C XML 架构构造是 XML 架构定义语言 (XSD) 架构的 <xref:System.Xml.Schema.XmlSchemaInference> 过程无法处理的，并会引发异常。 例如顶级复合器不是序列的复杂类型。 在架构对象模型 (SOM) 中，这相对于 <xref:System.Xml.Schema.XmlSchemaComplexType>，它的 <xref:System.Xml.Schema.XmlSchemaComplexType.Particle%2A> 属性不是 <xref:System.Xml.Schema.XmlSchemaSequence> 的实例。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.Schema.XmlSchemaInference>
- [XML 架构对象模型 (SOM)](xml-schema-object-model-som.md)
- [推断 XML 架构](inferring-an-xml-schema.md)
- [推断架构节点类型和结构的规则](rules-for-inferring-schema-node-types-and-structure.md)
- [推断简单类型的规则](rules-for-inferring-simple-types.md)
