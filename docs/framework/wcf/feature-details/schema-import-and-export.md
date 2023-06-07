---
title: 架构导入和导出
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, schema import and export
- XsdDataContractExporter class
- XsdDataContractImporter class
ms.assetid: 0da32b50-ccd9-463a-844c-7fe803d3bf44
ms.openlocfilehash: 52a9e1bf4c9442bd42beb55b133a185c4a42148d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96288560"
---
# <a name="schema-import-and-export"></a>架构导入和导出

Windows Communication Foundation (WCF) 包括新的序列化引擎 <xref:System.Runtime.Serialization.DataContractSerializer> 。 `DataContractSerializer`在两个方向) 之间转换 .NET Framework 对象和 XML (。 除了序列化程序本身，WCF 还包括关联的架构导入和架构导出机制。 *架构* 是序列化程序生成或反序列化程序可以访问的 XML 形状的正式、精确和计算机可读的说明。 WCF 使用万维网联合会 (W3C) XML 架构定义语言 (XSD) 作为其架构表示形式，这与许多第三方平台广泛互操作。  
  
 架构导入组件 <xref:System.Runtime.Serialization.XsdDataContractImporter> 使用 XSD 架构文档并生成 .NET Framework 类 (正常数据协定类) 以便序列化的窗体与给定的架构相对应。  
  
 例如，以下架构片段：  
  
 [!code-csharp[c_SchemaImportExport#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#8)]
 [!code-vb[c_SchemaImportExport#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#8)]  
  
 生成以下类型（略经简化以便于阅读）。  
  
 [!code-csharp[c_SchemaImportExport#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#1)]
 [!code-vb[c_SchemaImportExport#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#1)]  
  
 请注意，生成的类型遵循几个数据协定最佳做法 (在 [最佳做法中找到：数据协定版本控制](../best-practices-data-contract-versioning.md)) ：  
  
- 此类型实现 <xref:System.Runtime.Serialization.IExtensibleDataObject> 接口。 有关详细信息，请参阅[向前兼容的数据协定](forward-compatible-data-contracts.md)。  
  
- 数据成员作为封装私有字段的公共属性来实现。  
  
- 此类是一个分部类，不修改生成的代码就可以添加内容。  
  
 <xref:System.Runtime.Serialization.XsdDataContractExporter> 使您能够进行反向操作，使用可用 `DataContractSerializer` 进行序列化的类型并生成 XSD 架构文档。  
  
## <a name="fidelity-is-not-guaranteed"></a>不保证保真  

 不保证进行往返行程的架构或类型完全保真。  (*往返* 过程中，导入架构以创建一组类，并将结果导出为再次创建一个架构。可能无法返回相同的架构 ) 。 反向过程也不保证保真。 （导出类型以生成其架构，然后重新导入此类型。 不太可能返回相同的类型。）  
  
## <a name="supported-types"></a>支持的类型  

 数据协定模型只支持有限的 WC3 架构子集。 导入过程中任何不符合此子集的架构都将导致异常。 例如，没有办法可以指定数据协定的数据成员应该作为 XML 属性进行序列化。 这样，由于不能用正确的 XML 投影生成数据协定，需要使用 XML 属性的架构将不受支持，并且将在导入过程中导致异常。  
  
 例如，不能使用默认导入设置导入以下架构片段。  
  
 [!code-xml[c_SchemaImportExport#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/common/source.config#9)]  
  
 有关详细信息，请参阅 [数据协定架构参考](data-contract-schema-reference.md)。 如果架构不符合数据协定规则，请使用另一个序列化引擎。 例如，<xref:System.Xml.Serialization.XmlSerializer> 使用自己的独立架构导入机制。 另有一种特殊的导入模式，可在其中扩展所支持架构的范围。 有关详细信息，请参阅 <xref:System.Xml.Serialization.IXmlSerializable> 导入架构中的生成类型 [以生成类](importing-schema-to-generate-classes.md)部分。  
  
 `XsdDataContractExporter`支持可通过序列化的任何 .NET Framework 类型 `DataContractSerializer` 。 有关详细信息，请参阅 [数据协定序列化程序支持的类型](types-supported-by-the-data-contract-serializer.md)。 请注意，使用 `XsdDataContractExporter` 生成的架构通常是 `XsdDataContractImporter` 可以使用的有效数据（除非 <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> 用于对架构进行自定义）。  
  
 有关使用的详细信息 <xref:System.Runtime.Serialization.XsdDataContractImporter> ，请参阅 [导入架构以生成类](importing-schema-to-generate-classes.md)。  
  
 有关使用的详细信息 <xref:System.Runtime.Serialization.XsdDataContractExporter> ，请参阅 [从类导出架构](exporting-schemas-from-classes.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.XsdDataContractImporter>
- <xref:System.Runtime.Serialization.XsdDataContractExporter>
- [导入架构以生成类](importing-schema-to-generate-classes.md)
- [从类导出架构](exporting-schemas-from-classes.md)
