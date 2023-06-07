---
title: 后架构编译信息集
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
ms.assetid: 7f1bc7f4-401b-459f-9078-f099cc711fde
ms.openlocfilehash: 79ffea3831d5f56d73c6974fdf7265c9b00ab8ec
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95691261"
---
# <a name="post-schema-compilation-infoset"></a>后架构编译信息集

[万维网联合会 (W3C) XML 架构建议](https://www.w3.org/XML/Schema)介绍了必须公开的架构验证前和架构编译后信息集。 XML 架构对象模型 (SOM) 在调用 <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> 的 <xref:System.Xml.Schema.XmlSchemaSet> 方法之前或之后查看此信息集。  
  
 前架构验证信息集在编辑架构过程中生成。 后架构编译信息集在调用 <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> 的 <xref:System.Xml.Schema.XmlSchemaSet> 方法之后、编译架构期间生成，并以属性的形式公开。  
  
 SOM 是表示前架构验证和后架构编译信息集的对象模型；该模型由 <xref:System.Xml.Schema?displayProperty=nameWithType> 命名空间中的类组成。 <xref:System.Xml.Schema> 命名空间中的类的所有读取和写入属性属于前架构验证信息集，而 <xref:System.Xml.Schema> 命名空间中的类的所有只读属性属于后架构编译信息集。 此规则不适用于下列属性，下列属性既是前架构验证信息集属性也是后架构编译信息集属性。  
  
|类|Property|  
|-----------|--------------|  
|<xref:System.Xml.Schema.XmlSchemaObject>|<xref:System.Xml.Schema.XmlSchemaObject.Parent%2A>|  
|<xref:System.Xml.Schema.XmlSchema>|<xref:System.Xml.Schema.XmlSchema.AttributeFormDefault%2A>, <xref:System.Xml.Schema.XmlSchema.BlockDefault%2A>, <xref:System.Xml.Schema.XmlSchema.ElementFormDefault%2A>, <xref:System.Xml.Schema.XmlSchema.FinalDefault%2A>, <xref:System.Xml.Schema.XmlSchema.TargetNamespace%2A>|  
|<xref:System.Xml.Schema.XmlSchemaExternal>|<xref:System.Xml.Schema.XmlSchemaExternal.Schema%2A>|  
|<xref:System.Xml.Schema.XmlSchemaAttributeGroup>|<xref:System.Xml.Schema.XmlSchemaAttributeGroup.AnyAttribute%2A>|  
|<xref:System.Xml.Schema.XmlSchemaParticle>|<xref:System.Xml.Schema.XmlSchemaParticle.MaxOccurs%2A>，<xref:System.Xml.Schema.XmlSchemaParticle.MinOccurs%2A>|  
|<xref:System.Xml.Schema.XmlSchemaComplexType>|<xref:System.Xml.Schema.XmlSchemaComplexType.AnyAttribute%2A>|  
  
 例如，<xref:System.Xml.Schema.XmlSchemaElement> 和 <xref:System.Xml.Schema.XmlSchemaComplexType> 类均具有 `BlockResolved` 和 `FinalResolved` 属性。 在编译并验证了架构之后，这些属性用于存放 `Block` 和 `Final` 属性的值。 `BlockResolved` 和 `FinalResolved` 是属于后架构编译信息集的只读属性。  
  
 以下示例显示在验证架构之后设置的 <xref:System.Xml.Schema.XmlSchemaElement.ElementSchemaType%2A> 类的 <xref:System.Xml.Schema.XmlSchemaElement> 属性。 在验证之前，该属性包含 `null` 引用，<xref:System.Xml.Schema.XmlSchemaElement.SchemaTypeName%2A> 设置为相应类型的名称。 在验证之后，<xref:System.Xml.Schema.XmlSchemaElement.SchemaTypeName%2A> 被解析为有效类型，并且可以通过 <xref:System.Xml.Schema.XmlSchemaElement.ElementSchemaType%2A> 属性使用类型对象。  
  
 [!code-cpp[PsciSample#1](../../../../samples/snippets/cpp/VS_Snippets_Data/PsciSample/CPP/PsciSample.cpp#1)]
 [!code-csharp[PsciSample#1](../../../../samples/snippets/csharp/VS_Snippets_Data/PsciSample/CS/PsciSample.cs#1)]
 [!code-vb[PsciSample#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/PsciSample/VB/PsciSample.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [XML 架构对象模型 (SOM)](xml-schema-object-model-som.md)
