---
title: 生成 XML 架构
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
ms.assetid: 8a5ea56c-0140-4b51-8997-875ae6a8e0cb
ms.openlocfilehash: 177c52ea7217821735250b297974ac0ebaa1eb2e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725373"
---
# <a name="building-xml-schemas"></a>生成 XML 架构

<xref:System.Xml.Schema?displayProperty=nameWithType> 命名空间中的类映射到万维网联合会 (W3C) XML 架构建议中定义的结构，可以用于在内存中生成 XML 架构。  
  
## <a name="building-an-xml-schema"></a>生成 XML 架构  

 在下面的代码示例中，SOM API 用于在内存中生成客户 XML 架构。  
  
### <a name="creating-element-and-attributes"></a>创建元素和属性  

 代码示例从下至上生成客户架构，先创建子元素、属性及其相应的类型，然后创建顶级元素。  
  
 在下面的代码示例中，客户架构的 `FirstName` 和 `LastName` 元素以及 `CustomerId` 属性使用 SOM 的 <xref:System.Xml.Schema.XmlSchemaElement> 和 <xref:System.Xml.Schema.XmlSchemaAttribute> 类创建。 除了 <xref:System.Xml.Schema.XmlSchemaElement.Name%2A> 和 <xref:System.Xml.Schema.XmlSchemaElement> 类的 <xref:System.Xml.Schema.XmlSchemaAttribute> 属性（对应于 XML 架构中 `<xs:element />` 和 `<xs:attribute />` 元素的“name”属性）之外，架构所允许的所有其他属性（`defaultValue`、`fixedValue`、`form` 等）在 <xref:System.Xml.Schema.XmlSchemaElement> 和 <xref:System.Xml.Schema.XmlSchemaAttribute> 类中均有对应的属性。  
  
 [!code-cpp[XmlSchemaCreateExample#2](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaCreateExample/CPP/XmlSchemaCreateExample.cpp#2)]
 [!code-csharp[XmlSchemaCreateExample#2](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaCreateExample/CS/XmlSchemaCreateExample.cs#2)]
 [!code-vb[XmlSchemaCreateExample#2](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaCreateExample/VB/XmlSchemaCreateExample.vb#2)]  
  
### <a name="creating-schema-types"></a>创建架构类型  

 元素和属性的内容通过其类型定义。 要创建类型属于一种内置架构类型的元素和属性，<xref:System.Xml.Schema.XmlSchemaElement.SchemaTypeName%2A> 或 <xref:System.Xml.Schema.XmlSchemaElement> 类的 <xref:System.Xml.Schema.XmlSchemaAttribute> 属性应使用 <xref:System.Xml.XmlQualifiedName> 类设置该内置类型相应的限定名。 要为元素和属性创建用户定义类型，应使用 <xref:System.Xml.Schema.XmlSchemaSimpleType> 或 <xref:System.Xml.Schema.XmlSchemaComplexType> 类创建新的简单类型或复杂类型。  
  
> [!NOTE]
> 要创建未命名的简单类型或复杂类型，并且属于元素或属性的匿名子级（只有简单类型适用于属性），应将 <xref:System.Xml.Schema.XmlSchemaElement.SchemaType%2A> 或 <xref:System.Xml.Schema.XmlSchemaElement> 类的 <xref:System.Xml.Schema.XmlSchemaAttribute> 属性设置为未命名的简单类型或复杂类型，而不是设置 <xref:System.Xml.Schema.XmlSchemaElement.SchemaTypeName%2A> 或 <xref:System.Xml.Schema.XmlSchemaElement> 类的 <xref:System.Xml.Schema.XmlSchemaAttribute> 属性。  
  
 XML 架构允许通过限制从其他简单类型（内置或用户定义）派生的匿名和命名的简单类型，或作为其他简单类型的列表或联合构造的匿名和命名的简单类型。 <xref:System.Xml.Schema.XmlSchemaSimpleTypeRestriction> 类用于通过限制内置 `xs:string` 类型来创建简单类型。 也可以使用 <xref:System.Xml.Schema.XmlSchemaSimpleTypeList> 或 <xref:System.Xml.Schema.XmlSchemaSimpleTypeUnion> 类创建列表或联合类型。 <xref:System.Xml.Schema.XmlSchemaSimpleType.Content%2A?displayProperty=nameWithType> 属性表明属于简单类型限制、列表还是联合。  
  
 在下面的代码示例中，`FirstName` 元素的类型是内置类型 `xs:string`，`LastName` 元素的类型是命名的简单类型，属于内置类型 `xs:string` 的限制，`MaxLength` 方面的值为 20，`CustomerId` 属性的类型是内置类型 `xs:positiveInteger`。 `Customer` 元素是匿名的复杂类型，其粒子是 `FirstName` 和 `LastName` 元素的序列，其属性包含 `CustomerId` 属性。  
  
> [!NOTE]
> 也可以使用 <xref:System.Xml.Schema.XmlSchemaChoice> 或 <xref:System.Xml.Schema.XmlSchemaAll> 类作为复杂类型的粒子，以复制 `<xs:choice />` 或 `<xs:all />` 语义。  
  
 [!code-cpp[XmlSchemaCreateExample#3](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaCreateExample/CPP/XmlSchemaCreateExample.cpp#3)]
 [!code-csharp[XmlSchemaCreateExample#3](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaCreateExample/CS/XmlSchemaCreateExample.cs#3)]
 [!code-vb[XmlSchemaCreateExample#3](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaCreateExample/VB/XmlSchemaCreateExample.vb#3)]  
  
### <a name="creating-and-compiling-schemas"></a>创建和编译架构  

 现在，已使用 SOM API 在内存中创建了子元素和属性及其相应的类型以及顶级 `Customer` 元素。 在下面的代码示例中，架构元素使用 <xref:System.Xml.Schema.XmlSchema> 类创建，顶级元素和类型使用 <xref:System.Xml.Schema.XmlSchema.Items%2A?displayProperty=nameWithType> 属性添加到该元素，完整的架构使用 <xref:System.Xml.Schema.XmlSchemaSet> 类进行编译并写入控制台。  
  
 [!code-cpp[XmlSchemaCreateExample#4](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaCreateExample/CPP/XmlSchemaCreateExample.cpp#4)]
 [!code-csharp[XmlSchemaCreateExample#4](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaCreateExample/CS/XmlSchemaCreateExample.cs#4)]
 [!code-vb[XmlSchemaCreateExample#4](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaCreateExample/VB/XmlSchemaCreateExample.vb#4)]  
  
 <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A?displayProperty=nameWithType> 方法针对 XML 架构的规则验证客户架构，并使后架构编译属性可用。  
  
> [!NOTE]
> SOM API 中的所有后架构编译属性与后架构验证信息集不同。  
  
 添加到 <xref:System.Xml.Schema.ValidationEventHandler> 的 <xref:System.Xml.Schema.XmlSchemaSet> 是一个委托，用于调用回调方法 `ValidationCallback` 来处理架构验证警告和错误。  
  
 以下是完整的代码示例以及写入控制台的客户架构。  
  
 [!code-cpp[XmlSchemaCreateExample#1](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaCreateExample/CPP/XmlSchemaCreateExample.cpp#1)]
 [!code-csharp[XmlSchemaCreateExample#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaCreateExample/CS/XmlSchemaCreateExample.cs#1)]
 [!code-vb[XmlSchemaCreateExample#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaCreateExample/VB/XmlSchemaCreateExample.vb#1)]  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<xs:schema xmlns:tns="http://www.tempuri.org" targetNamespace="http://www.tempuri.org" xmlns:xs="http://www.w3.org/2001/XMLSchema">  
   <xs:element name="Customer">  
      <xs:complexType>  
         <xs:sequence>  
            <xs:element name="FirstName" type="xs:string" />  
            <xs:element name="LastName" type="tns:LastNameType" />  
         </xs:sequence>  
         <xs:attribute name="CustomerId" type="xs:positiveInteger" use="required" />  
      </xs:complexType>  
   </xs:element>  
   <xs:simpleType name="LastNameType">  
      <xs:restriction base="xs:string">  
         <xs:maxLength value="20" />  
      </xs:restriction>  
   </xs:simpleType>  
</xs:schema>  
```  
  
## <a name="see-also"></a>请参阅

- [XML 架构对象模型概述](xml-schema-object-model-overview.md)
- [读取和编写 XML 架构](reading-and-writing-xml-schemas.md)
- [遍历 XML 架构](traversing-xml-schemas.md)
- [编辑 XML 架构](editing-xml-schemas.md)
- [包含或导入 XML 架构](including-or-importing-xml-schemas.md)
- [用于编译架构的 XmlSchemaSet](xmlschemaset-for-schema-compilation.md)
- [后架构编译信息集](post-schema-compilation-infoset.md)
