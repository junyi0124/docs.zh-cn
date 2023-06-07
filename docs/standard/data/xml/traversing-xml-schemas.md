---
title: 遍历 XML 架构
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
ms.assetid: cce69574-5861-4a30-b730-2e18d915d8ee
ms.openlocfilehash: 6371d7e16af45eebf09f95bce2864be3bf44321e
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94824670"
---
# <a name="traversing-xml-schemas"></a>遍历 XML 架构

使用架构对象模型 (SOM) API 遍历 XML 架构，可以访问 SOM 中存储的元素、属性和类型。 遍历加载到 SOM 的 XML 架构也是使用 SOM API 编辑 XML 架构的第一步。

## <a name="traversing-an-xml-schema"></a>遍历 XML 架构

使用 <xref:System.Xml.Schema.XmlSchema> 类的下列属性可以访问添加到 XML 架构的所有全局项的集合。

|Property|存储在集合或数组中的对象类型|
|--------------|---------------------------------------------------|
|<xref:System.Xml.Schema.XmlSchema.Elements%2A>|<xref:System.Xml.Schema.XmlSchemaElement>|
|<xref:System.Xml.Schema.XmlSchema.Attributes%2A>|<xref:System.Xml.Schema.XmlSchemaAttribute>|
|<xref:System.Xml.Schema.XmlSchema.AttributeGroups%2A>|<xref:System.Xml.Schema.XmlSchemaAttributeGroup>|
|<xref:System.Xml.Schema.XmlSchema.Groups%2A>|<xref:System.Xml.Schema.XmlSchemaGroup>|
|<xref:System.Xml.Schema.XmlSchema.Includes%2A>|<xref:System.Xml.Schema.XmlSchemaExternal>、<xref:System.Xml.Schema.XmlSchemaInclude>、<xref:System.Xml.Schema.XmlSchemaImport> 或 <xref:System.Xml.Schema.XmlSchemaRedefine>|
|<xref:System.Xml.Schema.XmlSchema.Items%2A>|<xref:System.Xml.Schema.XmlSchemaObject>（提供对所有全局级元素、属性和类型的访问）|
|<xref:System.Xml.Schema.XmlSchema.Notations%2A>|<xref:System.Xml.Schema.XmlSchemaNotation>|
|<xref:System.Xml.Schema.XmlSchema.SchemaTypes%2A>|<xref:System.Xml.Schema.XmlSchemaType>, <xref:System.Xml.Schema.XmlSchemaSimpleType>, <xref:System.Xml.Schema.XmlSchemaComplexType>|
|<xref:System.Xml.Schema.XmlSchema.UnhandledAttributes%2A>|<xref:System.Xml.XmlAttribute>（提供对不属于架构命名空间的属性的访问）|

> [!NOTE]
> 上表列出的所有属性（<xref:System.Xml.Schema.XmlSchema.Items%2A> 属性除外）是后架构编译信息集 (PSCI) 属性，直到架构编译后才可用。 <xref:System.Xml.Schema.XmlSchema.Items%2A> 属性是前架构编译属性，可以在架构编译之前使用，以访问和编辑所有全局级元素、属性和类型。
>
> <xref:System.Xml.Schema.XmlSchema.UnhandledAttributes%2A> 属性提供对不属于架构命名空间的所有属性的访问。 架构处理器不处理这些属性。

随后的代码示例展示了如何遍历[生成 XML 架构](building-xml-schemas.md)主题中创建的客户架构。 代码示例演示如何使用上述集合遍历架构并将架构中的所有元素和属性写入控制台。

示例通过下列步骤遍历客户架构。

1. 将客户架构添加到新的 <xref:System.Xml.Schema.XmlSchemaSet> 对象并进行编译。 在读取或编译架构时遇到的任何架构验证警告和错误由 <xref:System.Xml.Schema.ValidationEventHandler> 委托进行处理。

2. 通过循环访问 <xref:System.Xml.Schema.XmlSchema> 属性，从 <xref:System.Xml.Schema.XmlSchemaSet> 中检索已编译的 <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> 对象。 因为架构已编译，所以，可以访问后架构编译信息集 (PSCI) 属性。

3. 在将每个元素的名称写入控制台的后架构编译 <xref:System.Xml.Schema.XmlSchemaElement> 集合的 <xref:System.Xml.Schema.XmlSchemaObjectTable.Values%2A> 集合中循环访问每个 <xref:System.Xml.Schema.XmlSchema.Elements%2A?displayProperty=nameWithType>。

4. 使用 `Customer` 类获取 <xref:System.Xml.Schema.XmlSchemaComplexType> 元素的复杂类型。

5. 如果复杂类型具有任何属性，请获取 <xref:System.Collections.IDictionaryEnumerator> 以循环访问每个 <xref:System.Xml.Schema.XmlSchemaAttribute> 并将其名称写入控制台。

6. 使用 <xref:System.Xml.Schema.XmlSchemaSequence> 类获取复杂类型的序列粒子。

7. 在将每个子元素的名称写入控制台的 <xref:System.Xml.Schema.XmlSchemaElement> 集合中循环访问每个 <xref:System.Xml.Schema.XmlSchemaSequence.Items%2A?displayProperty=nameWithType>。

以下是完整的代码示例。

[!code-cpp[XmlSchemaTraverseExample#1](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaTraverseExample/CPP/XmlSchemaTraverseExample.cpp#1)]
[!code-csharp[XmlSchemaTraverseExample#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaTraverseExample/CS/XmlSchemaTraverseExample.cs#1)]
[!code-vb[XmlSchemaTraverseExample#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaTraverseExample/VB/XmlSchemaTraverseExample.vb#1)]

如果是用户定义的简单类型或复杂类型，<xref:System.Xml.Schema.XmlSchemaElement.ElementSchemaType%2A?displayProperty=nameWithType> 属性可以为 <xref:System.Xml.Schema.XmlSchemaSimpleType> 或 <xref:System.Xml.Schema.XmlSchemaComplexType>。 如果是 W3C XML 架构建议中定义的一个内置数据类型，此属性也可以为 <xref:System.Xml.Schema.XmlSchemaDatatype>。 在客户架构中，<xref:System.Xml.Schema.XmlSchemaElement.ElementSchemaType%2A> 元素的 `Customer` 为 <xref:System.Xml.Schema.XmlSchemaComplexType>，`FirstName` 和 `LastName` 元素为 <xref:System.Xml.Schema.XmlSchemaSimpleType>。

[生成 XML 架构](building-xml-schemas.md)主题中的代码示例使用了 <xref:System.Xml.Schema.XmlSchemaComplexType.Attributes%2A?displayProperty=nameWithType> 集合，将属性 `CustomerId` 添加到 `Customer` 元素。 此属性是前架构编译属性。 对应的后架构编译信息集属性为 <xref:System.Xml.Schema.XmlSchemaComplexType.AttributeUses%2A?displayProperty=nameWithType> 集合，该集合包含复杂类型的所有属性，包括通过类型派生继承的属性。

## <a name="see-also"></a>请参阅

- [XML 架构对象模型概述](xml-schema-object-model-overview.md)
- [读取和编写 XML 架构](reading-and-writing-xml-schemas.md)
- [生成 XML 架构](building-xml-schemas.md)
- [编辑 XML 架构](editing-xml-schemas.md)
- [包含或导入 XML 架构](including-or-importing-xml-schemas.md)
- [用于编译架构的 XmlSchemaSet](xmlschemaset-for-schema-compilation.md)
- [后架构编译信息集](post-schema-compilation-infoset.md)
