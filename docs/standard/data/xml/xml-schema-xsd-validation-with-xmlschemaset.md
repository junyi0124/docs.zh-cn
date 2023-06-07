---
title: 使用 XmlSchemaSet 进行 XML 架构 (XSD) 验证
description: 了解如何使用 .NET 中的 XmlSchemaSet 类根据 XML 架构定义语言 (XSD) 架构验证 XML 文档。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
ms.assetid: 359b10eb-ec05-4cc6-ac96-c2b060afc4de
ms.openlocfilehash: 82944fd3fb97c3086ffd47fbd2ba1f3192e6deb4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672248"
---
# <a name="xml-schema-xsd-validation-with-xmlschemaset"></a>使用 XmlSchemaSet 进行 XML 架构 (XSD) 验证

XML 文档可以根据 <xref:System.Xml.Schema.XmlSchemaSet> 中的 XML 架构定义语言 (XSD) 架构进行验证。  
  
## <a name="validate-xml-documents"></a>验证 XML 文档  

 XML 文档通过 <xref:System.Xml.XmlReader.Create%2A> 类的 <xref:System.Xml.XmlReader> 方法进行验证。 若要验证 XML 文档，请构造一个 <xref:System.Xml.XmlReaderSettings> 对象，其中包含用于验证 XML 文档的 XML 架构定义语言 (XSD) 架构。  
  
> [!NOTE]
> <xref:System.Xml.Schema> 命名空间包含扩展方法，可用于在使用 [LINQ to XML (C#)](../../linq/linq-xml-overview.md) 和 [LINQ to XML (Visual Basic)](../../linq/linq-xml-overview.md) 时，轻松地针对 XSD 文件来验证 XML 树。 若要详细了解如何使用 LINQ to XML 验证 XML 文档，请参阅[如何使用 XSD (LINQ to XML) (C#) 进行验证](../../linq/validate-xsd.md)和[如何：使用 XSD (LINQ to XML) (Visual Basic) 进行验证](../../linq/validate-xsd.md)。
  
 通过将架构作为参数传递给 <xref:System.Xml.Schema.XmlSchemaSet> 的 <xref:System.Xml.Schema.XmlSchemaSet> 方法，可以将单个架构或一组架构（作为 <xref:System.Xml.Schema.XmlSchemaSet.Add%2A>）添加到 <xref:System.Xml.Schema.XmlSchemaSet>。 在验证文档时，文档的目标命名空间必须匹配架构集中架构的目标命名空间。  
  
 下面是示例 XML 文档。  
  
 [!code-xml[XSDInference Examples #5](../../../../samples/snippets/xml/VS_Snippets_Data/XSDInference Examples/XML/contosoBooks.xml#5)]  
  
 下面是验证示例 XML 文档的架构。  
  
 [!code-xml[XSDInference Examples #6](../../../../samples/snippets/xml/VS_Snippets_Data/XSDInference Examples/XML/contosoBooks.xsd#6)]  
  
 在下面的代码示例中，上面的架构添加到 <xref:System.Xml.XmlReaderSettings> 对象的 <xref:System.Xml.XmlReaderSettings.Schemas%2A> 属性中。 <xref:System.Xml.XmlReaderSettings> 对象作为参数传递给验证上述 XML 文档的 <xref:System.Xml.XmlReader.Create%2A> 对象的 <xref:System.Xml.XmlReader> 方法。  
  
 <xref:System.Xml.XmlReaderSettings.ValidationType%2A> 对象的 <xref:System.Xml.XmlReaderSettings> 属性设置为 `Schema`，强制通过 <xref:System.Xml.XmlReader.Create%2A> 对象的 <xref:System.Xml.XmlReader> 方法验证 XML 文档。 将 <xref:System.Xml.Schema.ValidationEventHandler> 添加到 <xref:System.Xml.XmlReaderSettings> 对象以处理 XML 文档和架构验证过程中发现的错误所引发的任何 <xref:System.Xml.Schema.XmlSeverityType.Warning> 或 <xref:System.Xml.Schema.XmlSeverityType.Error> 事件。  
  
 [!code-cpp[XmlSchemaSetOverall Example #1](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaSetOverall Example/CPP/xmlschemasetexample.cpp#1)]
 [!code-csharp[XmlSchemaSetOverall Example #1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaSetOverall Example/CS/xmlschemasetexample.cs#1)]
 [!code-vb[XmlSchemaSetOverall Example #1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaSetOverall Example/VB/xmlschemasetexample.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [用于编译架构的 XmlSchemaSet](xmlschemaset-for-schema-compilation.md)
- [使用 XML 架构](working-with-xml-schemas.md)
