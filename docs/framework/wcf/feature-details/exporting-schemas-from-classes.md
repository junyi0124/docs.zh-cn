---
title: 从类导出架构
ms.date: 03/30/2017
helpviewer_keywords:
- WCF, schema import and export
- schemas [WCF], exporting from classes
- schemas [WCF]
- XsdDataContractExporter class
- XsdDataContractImporter class
ms.assetid: bb57b962-70c1-45a9-93d5-e721e340a13f
ms.openlocfilehash: 8105ada41a2ec9dac962f5029ac89b66c69893ab
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96255554"
---
# <a name="exporting-schemas-from-classes"></a>从类导出架构

若要从数据协定模型中使用的类生成 XML 架构定义语言 (XSD) 架构，请使用 <xref:System.Runtime.Serialization.XsdDataContractExporter> 类。 本主题描述创建架构的过程。  
  
## <a name="the-export-process"></a>导出过程  

 架构导出过程以一个或多个类型开始，并生成一个 <xref:System.Xml.Schema.XmlSchemaSet> ，描述这些类型的 XML 投影。  
  
 `XmlSchemaSet`是 .NET Framework 的架构对象模型的一部分 (SOM) 表示一组 XSD 架构文档。 若要从 `XmlSchemaSet`创建 XSD 文档，请使用 <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> 类的 `XmlSchemaSet` 属性的架构集合。 然后使用 <xref:System.Xml.Schema.XmlSchema> 序列化每个 <xref:System.Xml.Serialization.XmlSerializer>对象。  
  
#### <a name="to-export-schemas"></a>导出架构  
  
1. 创建 <xref:System.Runtime.Serialization.XsdDataContractExporter> 的实例：  
  
2. 可选。 在构造函数中传递一个 <xref:System.Xml.Schema.XmlSchemaSet> 。 在此情况下，会将架构导出期间生成的架构添加到此 <xref:System.Xml.Schema.XmlSchemaSet> 实例中，而不是以空白 <xref:System.Xml.Schema.XmlSchemaSet>开始。  
  
3. 可选。 调用 <xref:System.Runtime.Serialization.XsdDataContractExporter.CanExport%2A> 方法之一。 该方法确定是否可以导出指定类型。 该方法与下一个步骤中的 `Export` 方法具有相同的重载。  
  
4. 调用 <xref:System.Runtime.Serialization.XsdDataContractExporter.Export%2A> 方法之一。 有三个重载采用了 <xref:System.Type>、 <xref:System.Collections.Generic.List%601> 对象的 `Type` ，或 <xref:System.Collections.Generic.List%601> 对象的 <xref:System.Reflection.Assembly> 。 在最后一种情况中，将导出所有给定程序集中的所有类型。  
  
     多次调用 `Export` 方法将导致多个项被添加到同一个 `XmlSchemaSet`中。 如果某个类型已经存在于 `XmlSchemaSet` 中，则不会再在其中生成该类型。 因此，在同一 `Export` 上多次调用 `XsdDataContractExporter` 时最好创建 `XsdDataContractExporter` 类的多个实例。 这可避免生成重复的架构类型。  
  
    > [!NOTE]
    > 如果导出期间出现故障，则 `XmlSchemaSet` 将处于不可预知的状态。  
  
5. 通过 <xref:System.Xml.Schema.XmlSchemaSet> 属性访问 <xref:System.Runtime.Serialization.XsdDataContractExporter.Schemas%2A> 。  
  
## <a name="export-options"></a>导出选项  

 您可以将 <xref:System.Runtime.Serialization.XsdDataContractExporter.Options%2A> 的 <xref:System.Runtime.Serialization.XsdDataContractExporter> 属性设置为 <xref:System.Runtime.Serialization.ExportOptions> 类的实例以控制导出过程的各个方面。 您可以具体设置以下选项：  
  
- <xref:System.Runtime.Serialization.ExportOptions.KnownTypes%2A>. 这一 `Type` 集合表示要导出的类型的已知类型。  (有关详细信息，请参阅[数据协定已知类型](data-contract-known-types.md)。 `Export` 除了传递到方法的类型外，还会在每次调用时导出这些已知类型 ) 。 `Export`  
  
- <xref:System.Runtime.Serialization.ExportOptions.DataContractSurrogate%2A>. 通过此属性可以提供 <xref:System.Runtime.Serialization.IDataContractSurrogate> ，该属性将自定义导出过程。 有关详细信息，请参阅 [数据协定代理](../extending/data-contract-surrogates.md)项。 默认情况下，不使用代理项。  
  
## <a name="helper-methods"></a>帮助器方法  

 除了具有导出架构的主要作用， `XsdDataContractExporter` 还具有几个有用的帮助器方法，它们可提供有关类型的信息。 其中包括:  
  
- <xref:System.Runtime.Serialization.XsdDataContractExporter.GetRootElementName%2A> 方法。 此方法采用 `Type` 并返回表示根元素名称和命名空间的 <xref:System.Xml.XmlQualifiedName> ，此名称和命名空间可在此类型被序列化为根对象时使用。  
  
- <xref:System.Runtime.Serialization.XsdDataContractExporter.GetSchemaTypeName%2A> 方法。 此方法采用 `Type` 并返回表示 XSD 架构类型名称的 <xref:System.Xml.XmlQualifiedName> ，此名称可在此类型导出到架构中时使用。 对于表示架构中匿名类型的 <xref:System.Xml.Serialization.IXmlSerializable> 类型，此方法返回 `null`。  
  
- <xref:System.Runtime.Serialization.XsdDataContractExporter.GetSchemaType%2A> 方法。 此方法只能与表示架构中匿名类型的 <xref:System.Xml.Serialization.IXmlSerializable> 类型一起使用，并为所有其他类型返回 `null` 。 对于匿名类型，此方法返回表示给定 <xref:System.Xml.Schema.XmlSchemaType> 的 `Type`。  
  
 导出选项会影响所有这些方法。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.XsdDataContractImporter>
- <xref:System.Runtime.Serialization.XsdDataContractExporter>
- [架构导入和导出](schema-import-and-export.md)
- [导入架构以生成类](importing-schema-to-generate-classes.md)
