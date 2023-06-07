---
title: 写入数据集内容作为 XML 数据
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: fd15f8a5-3b4c-46d0-a561-4559ab2a4705
ms.openlocfilehash: c8a5c747e4ec60fcb97edf631aa3a0ae184ffec5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173713"
---
# <a name="writing-dataset-contents-as-xml-data"></a>写入数据集内容作为 XML 数据

在 ADO.NET 中可以编写 <xref:System.Data.DataSet> 的 XML 表示形式（包含或不包含其架构）。 如果架构信息以内联形式包含在 XML 表示形式中，则使用 XML 架构定义语言 (XSD) 来编写。 架构包含 <xref:System.Data.DataSet> 的表定义以及关系和约束定义。  
  
 当以 XML 数据形式编写 <xref:System.Data.DataSet> 时，<xref:System.Data.DataSet> 中的行将以它们的当前版本编写。 不过，也可以用 DiffGram 形式编写 <xref:System.Data.DataSet>，从而使行的当前值和原始值都将包含在内。  
  
 的 XML 表示形式 <xref:System.Data.DataSet> 可以写入文件、流、 **XmlWriter**或字符串。 这些选择在如何传输 <xref:System.Data.DataSet> 的 XML 表示形式方面提供了很大的灵活性。 若要以字符串的形式获取的 XML 表示形式 <xref:System.Data.DataSet> ，请使用 **GetXml** 方法，如以下示例中所示。  
  
```vb  
Dim xmlDS As String = custDS.GetXml()  
```  
  
```csharp  
string xmlDS = custDS.GetXml();  
```  
  
 **GetXml** 返回的 XML 表示形式， <xref:System.Data.DataSet> 没有架构信息。 若要将 (中的架构信息 <xref:System.Data.DataSet> 作为 XML 架构) 写入字符串，请使用 **GetXmlSchema**。  
  
 若要将写入 <xref:System.Data.DataSet> 文件、流或 **XmlWriter**，请使用 **WriteXml** 方法。 传递给 **WriteXml** 的第一个参数是 XML 输出的目标。 例如，传递包含文件名、一个 **system.object** 对象的字符串，等等。 可以传递 **XmlWriteMode** 的可选的第二个参数，以指定 XML 输出的写入方式。  
  
 下表显示了 **XmlWriteMode**的选项。  
  
|XmlWriteMode 选项|描述|  
|-------------------------|-----------------|  
|**IgnoreSchema**|以 XML 数据形式编写 <xref:System.Data.DataSet> 的当前内容，不包含 XML 架构。 这是默认设置。|  
|**WriteSchema**|以 XML 数据形式编写 <xref:System.Data.DataSet> 的当前内容，以关系结构作为内联 XML 架构。|  
|**DiffGram**|以 DiffGram 形式编写整个 <xref:System.Data.DataSet>，包括原始值和当前值。 有关详细信息，请参阅 [diffgram](diffgrams.md)。|  
  
 写入包含 DataRelation 对象的的 XML 表示形式时 <xref:System.Data.DataSet> ，您很可能希望所生成的 xml 将每个关系的子行嵌套在它们的相关父元素中**DataRelation** 。 若要实现此目的，请在向添加**datarelation**时将**datarelation**的**Nested**属性设置为**true** <xref:System.Data.DataSet> 。 有关详细信息，请参阅 [嵌套 datarelation](nesting-datarelations.md)。  
  
 下面两个示例演示如何将的 XML 表示形式写入 <xref:System.Data.DataSet> 文件。 第一个示例将生成的 XML 的文件名传递给 **WriteXml**。 第二个示例传递一个 **StreamWriter** 对象。
  
```vb  
custDS.WriteXml("Customers.xml", XmlWriteMode.WriteSchema)  
```  
  
```csharp  
custDS.WriteXml("Customers.xml", XmlWriteMode.WriteSchema);  
```  
  
```vb  
Dim xmlSW As System.IO.StreamWriter = New System.IO.StreamWriter("Customers.xml")  
custDS.WriteXml(xmlSW, XmlWriteMode.WriteSchema)  
xmlSW.Close()  
```  
  
```csharp  
System.IO.StreamWriter xmlSW = new System.IO.StreamWriter("Customers.xml");  
custDS.WriteXml(xmlSW, XmlWriteMode.WriteSchema);  
xmlSW.Close();  
```  
  
## <a name="mapping-columns-to-xml-elements-attributes-and-text"></a>将列映射到 XML 元素、属性和文本  

 您可以使用**DataColumn**对象的**ColumnMapping**属性来指定如何在 XML 中表示表的列。 下表显示了表列的**ColumnMapping**属性的不同**mappingtype.attribute**值以及生成的 XML。  
  
|MappingType 值|描述|  
|-----------------------|-----------------|  
|**元素**|这是默认设置。 列以元素名称为 ColumnName 的 XML 元素形式编写，列的内容以元素文本形式编写。 例如：<br /><br /> `<ColumnName>Column Contents</ColumnName>`|  
|**Attribute**|对于属性名称为 ColumnName 的当前行，列以 XML 元素的 XML 属性形式编写，列的内容以属性值形式编写。 例如：<br /><br /> `<RowElement ColumnName="Column Contents" />`|  
|**SimpleContent**|列的内容以 XML 元素中当前行文本的形式编写。 例如：<br /><br /> `<RowElement>Column Contents</RowElement>`<br /><br /> 请注意，无法为具有**元素**列或嵌套关系的表的列设置**SimpleContent** 。|  
|**Hidden**|不在 XML 输出中编写该列。|  
  
## <a name="see-also"></a>请参阅

- [在数据集中使用 XML](using-xml-in-a-dataset.md)
- [DiffGrams](diffgrams.md)
- [嵌套 DataRelation](nesting-datarelations.md)
- [写入数据集架构信息作为 XSD](writing-dataset-schema-information-as-xsd.md)
- [数据集、数据表和数据视图](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
