---
title: 从 XML 加载数据集
description: 了解如何从 XML 将内容添加到 ADO.NET 数据集。 .NET Framework 提供了对要加载的内容和数据集结构的灵活性。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 49c083b7-a5ed-41cf-aabc-5aaba96f00e6
ms.openlocfilehash: 0920acac2c82677cfce37703b7027dedce91a535
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91166802"
---
# <a name="loading-a-dataset-from-xml"></a>从 XML 加载数据集

ADO.NET <xref:System.Data.DataSet> 的内容可以从 XML 流或文档创建。 此外，利用 .NET Framework，您可以相当灵活地控制从 XML 中加载哪些信息以及如何创建 <xref:System.Data.DataSet> 的架构（即关系结构）。  
  
 若要使用 <xref:System.Data.DataSet> XML 中的数据填充，请使用对象的 **ReadXml** 方法 <xref:System.Data.DataSet> 。 **ReadXml**方法从文件、流或**XmlReader**中进行读取，并将 XML 的源以及可选的**XmlReadMode**参数作为参数。 有关 **XmlReader**的详细信息，请参阅 [用 XmlTextReader 读取 XML 数据](/previous-versions/dotnet/netframework-4.0/tfz3cz6w(v=vs.100))。 **ReadXml**方法读取 XML 流或文档的内容，并加载 <xref:System.Data.DataSet> 带有数据的。 它还会根据指定的 XmlReadMode 创建的关系架构 <xref:System.Data.DataSet> ， **XmlReadMode**以及关系架构是否已存在。  
  
 下表描述了 **XmlReadMode** 参数的选项。  
  
|选项|说明|  
|------------|-----------------|  
|**Auto**|这是默认设置。 检查 XML 并按如下顺序选择最适合的选项：<br /><br /> -如果 XML 为 DiffGram，则使用 **diffgram** 。<br />-如果 <xref:System.Data.DataSet> 包含架构或 XML 包含内联架构，则使用 **ReadSchema** 。<br />-如果不 <xref:System.Data.DataSet> 包含架构并且 XML 不包含内联架构，则使用 **InferSchema** 。<br /><br /> 如果知道要读取的 XML 的格式，则为了获得最佳性能，建议设置显式 **XmlReadMode**，而不是接受 **自动** 默认值。|  
|**ReadSchema**|读取内联架构并加载数据和架构。<br /><br /> 如果 <xref:System.Data.DataSet> 已包含架构，则新表将从内联架构添加到 <xref:System.Data.DataSet> 中的现有架构。 如果 <xref:System.Data.DataSet> 中已存在内联架构中的任何表，则将引发异常。 您将不能使用 **XmlReadMode**修改现有表的架构。<br /><br /> 如果 <xref:System.Data.DataSet> 不包含架构，并且没有内联架构，则不会读取任何数据。<br /><br /> 内联架构可以使用 XML 架构定义语言 (XSD) 架构来定义。 有关将内联架构作为 XML 架构写入的详细信息，请参阅 [从 Xml 架构派生数据集关系结构 (XSD) ](deriving-dataset-relational-structure-from-xml-schema-xsd.md)。|  
|**IgnoreSchema**|忽略任何内联架构并将数据加载到现有的 <xref:System.Data.DataSet> 架构中。 任何与现有架构不匹配的数据都将被丢弃。 如果 <xref:System.Data.DataSet> 中不存在任何架构，则不会加载任何数据。<br /><br /> 如果数据为 DiffGram，则**IgnoreSchema**具有与 diffgram 相同的功能**DiffGram** *。*|  
|**InferSchema**|忽略任何内联架构并按照 XML 数据的结构推断架构，然后加载数据。<br /><br /> 如果 <xref:System.Data.DataSet> 已经包含架构，则会通过向现有表中添加列来扩展当前架构。 如果不存在表，则不会添加其他表。 如果已存在具有不同命名空间的推断表或者任何推断表与现有列发生冲突，则会引发异常。<br /><br /> 有关 **ReadXmlSchema** 如何从 xml 文档推断架构的详细信息，请参阅 [从 Xml 推断数据集关系结构](inferring-dataset-relational-structure-from-xml.md)。|  
|**DiffGram**|读取 DiffGram 并将数据添加到当前架构中。 **DiffGram** 合并具有唯一标识符值匹配的现有行的新行。 请参见本主题末尾的“合并 XML 中的数据”。 有关 Diffgram 的详细信息，请参阅 [diffgram](diffgrams.md)。|  
|**Fragment**|持续读取多个 XML 片断，直至到达流的末尾。 与 <xref:System.Data.DataSet> 架构匹配的片断会追加到相应的表。 与 <xref:System.Data.DataSet> 架构不匹配的片断将被丢弃。|  
  
> [!NOTE]
> 如果将 **XmlReader** 传递给 **ReadXml** ，并且将其定位到 XML 文档中，则 **ReadXml** 将读取到下一个元素节点，并将其视为根元素，一直读取到元素节点的末尾。 如果指定 **XmlReadMode**，则此功能不适用。  
  
## <a name="dtd-entities"></a>DTD 实体  

 如果 XML 包含在文档类型定义 (DTD) 架构中定义的实体，则当您尝试 <xref:System.Data.DataSet> 通过将文件名、流或非验证 **XmlReader** 传递到 **ReadXml**来加载时，将引发异常。 相反，必须创建 **XmlValidatingReader**，并将 **EntityHandling** 设置为 **entityhandling.expandentities**，并将 **XmlValidatingReader** 传递到 **ReadXml**。 在读取实体之前， **XmlValidatingReader** 会将其展开 <xref:System.Data.DataSet> 。  
  
 以下代码示例显示如何从 XML 流中加载 <xref:System.Data.DataSet>。 第一个示例显示了传递给 **ReadXml** 方法的文件名。 第二个示例显示一个字符串，包含使用 <xref:System.IO.StringReader> 加载的 XML。  
  
```vb  
Dim dataSet As DataSet = New DataSet  
dataSet.ReadXml("input.xml", XmlReadMode.ReadSchema)  
```  
  
```csharp  
DataSet dataSet = new DataSet();  
dataSet.ReadXml("input.xml", XmlReadMode.ReadSchema);  
```  
  
```vb  
Dim dataSet As DataSet = New DataSet  
Dim dataTable As DataTable = New DataTable("table1")  
dataTable.Columns.Add("col1", Type.GetType("System.String"))  
dataSet.Tables.Add(dataTable)  
  
Dim xmlData As String = "<XmlDS><table1><col1>Value1</col1></table1><table1><col1>Value2</col1></table1></XmlDS>"  
  
Dim xmlSR As System.IO.StringReader = New System.IO.StringReader(xmlData)  
  
dataSet.ReadXml(xmlSR, XmlReadMode.IgnoreSchema)  
```  
  
```csharp  
DataSet dataSet = new DataSet();  
DataTable dataTable = new DataTable("table1");  
dataTable.Columns.Add("col1", typeof(string));  
dataSet.Tables.Add(dataTable);  
  
string xmlData = "<XmlDS><table1><col1>Value1</col1></table1><table1><col1>Value2</col1></table1></XmlDS>";  
  
System.IO.StringReader xmlSR = new System.IO.StringReader(xmlData);  
  
dataSet.ReadXml(xmlSR, XmlReadMode.IgnoreSchema);  
```  
  
> [!NOTE]
> 如果调用 **ReadXml** 来加载非常大的文件，则可能会遇到性能下降的情况。 若要确保 **ReadXml**的最佳性能，请在大文件上 <xref:System.Data.DataTable.BeginLoadData%2A> 为中的每个表调用方法， <xref:System.Data.DataSet> 然后调用 **ReadXml**。 最后，为 <xref:System.Data.DataTable.EndLoadData%2A> 中的每个表调用 <xref:System.Data.DataSet>，如下面的示例所示。  
  
```vb  
Dim dataTable As DataTable  
  
For Each dataTable In dataSet.Tables  
   dataTable.BeginLoadData()  
Next  
  
dataSet.ReadXml("file.xml")  
  
For Each dataTable in dataSet.Tables  
   dataTable.EndLoadData()  
Next  
```  
  
```csharp  
foreach (DataTable dataTable in dataSet.Tables)  
   dataTable.BeginLoadData();  
  
dataSet.ReadXml("file.xml");
  
foreach (DataTable dataTable in dataSet.Tables)  
   dataTable.EndLoadData();  
```  
  
> [!NOTE]
> 如果的 XSD 架构 <xref:System.Data.DataSet> 包含 **targetNamespace**，则数据可能不会被读取，并且你可能会在调用 **ReadXml** 以加载 <xref:System.Data.DataSet> 包含无限定命名空间的元素的 XML 时遇到异常。 若要在这种情况下读取未限定的元素，请在 XSD 架构中将 **elementFormDefault** 设置为等于 "合格的"。 例如：  
  
```xml  
<xsd:schema id="customDataSet"
  elementFormDefault="qualified"  
  targetNamespace="http://www.tempuri.org/customDataSet.xsd"
  xmlns="http://www.tempuri.org/customDataSet.xsd"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">  
</xsd:schema>  
```  
  
## <a name="merging-data-from-xml"></a>合并 XML 中的数据  

 如果 <xref:System.Data.DataSet> 已经包含数据，则会向已存在于 <xref:System.Data.DataSet> 中的数据添加 XML 中的新数据。 **ReadXml** 不会从 XML 合并到 <xref:System.Data.DataSet> 具有匹配主键的任何行信息。 若要使用 XML 中的新信息覆盖现有行信息，请使用 **ReadXml** 创建新的 <xref:System.Data.DataSet> ，然后将 <xref:System.Data.DataSet.Merge%2A> 新的 <xref:System.Data.DataSet> 引入现有 <xref:System.Data.DataSet> 。 请注意，使用**ReadXML**和**diffgram**的**XmlReadMode**加载 diffgram 将合并具有相同唯一标识符的行。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataSet.Merge%2A?displayProperty=nameWithType>
- [在数据集中使用 XML](using-xml-in-a-dataset.md)
- [DiffGrams](diffgrams.md)
- [从 XML 架构派生数据集关系结构 (XSD)](deriving-dataset-relational-structure-from-xml-schema-xsd.md)
- [从 XML 推断数据集关系结构](inferring-dataset-relational-structure-from-xml.md)
- [从 XML 加载数据集架构信息](loading-dataset-schema-information-from-xml.md)
- [数据集、数据表和数据视图](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
