---
title: 包含或导入 XML 架构
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
ms.assetid: fe1b4a11-37f4-4e1a-93c9-239f4fe736c0
ms.openlocfilehash: b2feb14e304c8ba7ecc19b2e79b943204044da05
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722669"
---
# <a name="including-or-importing-xml-schemas"></a>包含或导入 XML 架构

XML 架构可以包含 `<xs:import />`、`<xs:include />` 和 `<xs:redefine />` 元素。 这些架构元素引用其他 XML 架构，可以用于补充包括或导入这些架构的架构的结构。 <xref:System.Xml.Schema.XmlSchemaImport>、<xref:System.Xml.Schema.XmlSchemaInclude> 和 <xref:System.Xml.Schema.XmlSchemaRedefine> 类映射到架构对象模型 (SOM) API 中的这些元素。  
  
## <a name="including-or-importing-an-xml-schema"></a>包括或导入 XML 架构  

 下面的代码示例为[生成 XML 架构](building-xml-schemas.md)主题中创建的客户架构补充了地址架构。 为客户架构补充地址架构后，可以在客户架构中使用地址类型。  
  
 地址架构可以使用 `<xs:include />` 或 `<xs:import />` 元素加入，以原样使用地址架构的组件，也可以使用 `<xs:redefine />` 元素修改其任意组件，以适合客户架构的需要。 因为地址架构的 `targetNamespace` 与客户架构的不同，所以，将使用 `<xs:import />` 元素以及导入语义。  
  
 代码示例通过下列步骤包括客户架构。  
  
1. 将客户架构和地址架构添加到新的 <xref:System.Xml.Schema.XmlSchemaSet> 对象并进行编译。 _在读取或编译架构时遇到的任何架构验证警告和错误由 <xref:System.Xml.Schema.ValidationEventHandler> 委托进行处理。  
  
2. 通过循环访问 <xref:System.Xml.Schema.XmlSchema> 属性，从 <xref:System.Xml.Schema.XmlSchemaSet> 中为客户架构和地址架构检索已编译的 <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> 对象。 因为架构已编译，所以，可以访问后架构编译信息集 (PSCI) 属性。  
  
3. 创建一个 <xref:System.Xml.Schema.XmlSchemaImport> 对象，将导入的 <xref:System.Xml.Schema.XmlSchemaImport.Namespace%2A> 属性设置为地址架构的命名空间，将导入的 <xref:System.Xml.Schema.XmlSchemaExternal.Schema%2A> 属性设置为地址架构的 <xref:System.Xml.Schema.XmlSchema> 对象，并将导入添加到客户架构的 <xref:System.Xml.Schema.XmlSchema.Includes%2A> 属性中。  
  
4. 使用 <xref:System.Xml.Schema.XmlSchema> 类的 <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> 和 <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> 方法重新处理并编译已修改的客户架构的 <xref:System.Xml.Schema.XmlSchemaSet> 对象并将其写入控制台。  
  
5. 最后，使用客户架构的 <xref:System.Xml.Schema.XmlSchema.Includes%2A> 方法递归式将导入到客户架构的所有架构写入控制台。 <xref:System.Xml.Schema.XmlSchema.Includes%2A> 属性提供对所有添加到架构中的包括、导入或重新定义的访问。  
  
 以下是完整的代码示例以及写入控制台的客户架构和地址架构。  
  
 [!code-cpp[XmlSchemaImportExample#1](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaImportExample/CPP/XmlSchemaImportExample.cpp#1)]
 [!code-csharp[XmlSchemaImportExample#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaImportExample/CS/XmlSchemaImportExample.cs#1)]
 [!code-vb[XmlSchemaImportExample#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaImportExample/VB/XmlSchemaImportExample.vb#1)]  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<xs:schema xmlns:tns="http://www.tempuri.org" targetNamespace="http://www.tempuri.org" xmlns:xs="http://www.w3.org/2001/XMLSchema">  
  <xs:import namespace="http://www.example.com/IPO" />  
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
<schema targetNamespace="http://www.example.com/IPO" xmlns="http://www.w3.org/2001/XMLSchema" xmlns:ipo="http://www.example.com/IPO">  
  <annotation>  
    <documentation xml:lang="en">  
      Addresses for International Purchase order schema  
      Copyright 2000 Example.com. All rights reserved.  
    </documentation>  
  </annotation>  
  <complexType name="Address">  
    <sequence>  
      <element name="name"   type="string"/>  
      <element name="street" type="string"/>  
      <element name="city"   type="string"/>  
    </sequence>  
  </complexType>  
  <complexType name="USAddress">  
    <complexContent>  
      <extension base="ipo:Address">  
        <sequence>  
          <element name="state" type="ipo:USState"/>  
          <element name="zip"   type="positiveInteger"/>  
        </sequence>  
      </extension>  
    </complexContent>  
  </complexType>  
  <!-- other Address derivations for more countries or regions -->  
  <simpleType name="USState">  
    <restriction base="string">  
      <enumeration value="AK"/>  
      <enumeration value="AL"/>  
      <enumeration value="AR"/>  
      <!-- and so on ... -->  
    </restriction>  
  </simpleType>  
</schema>  
```  
  
 若要详细了解 `<xs:import />`、`<xs:include />` 和 `<xs:redefine />` 元素以及 <xref:System.Xml.Schema.XmlSchemaImport>、<xref:System.Xml.Schema.XmlSchemaInclude> 和 <xref:System.Xml.Schema.XmlSchemaRedefine> 类，请参阅 [W3C XML 架构](https://www.w3.org/XML/Schema)和 <xref:System.Xml.Schema?displayProperty=nameWithType> 命名空间类参考文档。  
  
## <a name="see-also"></a>请参阅

- [XML 架构对象模型概述](xml-schema-object-model-overview.md)
- [读取和编写 XML 架构](reading-and-writing-xml-schemas.md)
- [生成 XML 架构](building-xml-schemas.md)
- [遍历 XML 架构](traversing-xml-schemas.md)
- [编辑 XML 架构](editing-xml-schemas.md)
- [用于编译架构的 XmlSchemaSet](xmlschemaset-for-schema-compilation.md)
