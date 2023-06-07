---
title: 从 XML 加载数据集架构信息
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 43dfb23b-5cef-46f2-8d87-78f0fba1eb8c
ms.openlocfilehash: b084590d7158024227a9f12da759b56ae2031373
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201338"
---
# <a name="loading-dataset-schema-information-from-xml"></a>从 XML 加载数据集架构信息

<xref:System.Data.DataSet>可以通过编程方式定义 (其表、列、关系和约束) 的架构，也可以通过的**Fill**或**FillSchema**方法创建 <xref:System.Data.Common.DataAdapter> ，或从 XML 文档加载。 若要从 XML 文档加载**数据集**架构信息，可以使用**数据集**的**ReadXmlSchema**或**InferXmlSchema**方法。 通过**ReadXmlSchema** ，您可以从包含 XML 架构定义语言 (XSD) 架构的文档或使用内联 xml 架构的 xml 文档中加载或推断**数据集**架构信息。 **InferXmlSchema** 允许从 xml 文档推断架构，同时忽略指定的某些 XML 命名空间。  
  
> [!NOTE]
> 使用 Web 服务或 XML 序列化传输在内存中创建的**数据集**时，可能不会保留**数据集中**的表排序 (如嵌套关系) 。 因此，在这种情况下， **数据集** 的接收方不应依赖于表排序。 但是，如果要传输的 **数据集** 的架构是从 XSD 文件中读取的，而不是在内存中创建的，则始终保留表排序。  
  
## <a name="readxmlschema"></a>ReadXmlSchema  

 若要从 XML 文档加载数据**集**的架构而不加载任何数据，则可以使用该**数据集**的**ReadXmlSchema**方法。 **ReadXmlSchema** 创建使用 XML 架构定义语言 (XSD) 架构定义的 **数据集** 架构。  
  
 **ReadXmlSchema**方法获取文件名、流或包含要加载的 XML 文档的**XmlReader**的单个自变量。 XML 文档可以仅包含架构，也可以包含与包含数据的 XML 元素内联的架构。 有关将内联架构作为 XML 架构写入的详细信息，请参阅 [从 Xml 架构派生数据集关系结构 (XSD) ](deriving-dataset-relational-structure-from-xml-schema-xsd.md)。  
  
 如果传递到 **ReadXmlSchema** 的 XML 文档不包含内联架构信息，则 **READXMLSCHEMA** 将从 XML 文档中的元素推断架构。 如果 **数据集** 已包含架构，则将通过添加新表来扩展当前架构（如果它们尚不存在）。 不会将新列添加到现有表中。 如果要添加的列已存在于 **数据集中** ，但其类型与 XML 中的列不兼容，则会引发异常。 有关 **ReadXmlSchema** 如何从 xml 文档推断架构的详细信息，请参阅 [从 Xml 推断数据集关系结构](inferring-dataset-relational-structure-from-xml.md)。  
  
 尽管**ReadXmlSchema**只加载或推断**数据集**的架构，但**数据集**的**ReadXml**方法会加载或推断 XML 文档中包含的架构和数据。 有关详细信息，请参阅 [从 XML 加载数据集](loading-a-dataset-from-xml.md)。  
  
 下面的代码示例演示如何从 XML 文档或流加载 **数据集** 架构。 第一个示例显示了传递给 **ReadXmlSchema** 方法的 XML 架构文件名。 第二个示例显示了 **StreamReader**。  
  
```vb  
Dim dataSet As DataSet = New DataSet  
dataSet.ReadXmlSchema("schema.xsd")  
```  
  
```csharp  
DataSet dataSet = new DataSet();  
dataSet.ReadXmlSchema("schema.xsd");  
```  
  
```vb  
Dim xmlStream As New System.IO.StreamReader("schema.xsd")
Dim dataSet As DataSet = New DataSet  
dataSet.ReadXmlSchema(xmlStream)  
xmlStream.Close()  
```  
  
```csharp  
System.IO.StreamReader xmlStream = new System.IO.StreamReader("schema.xsd");  
DataSet dataSet = new DataSet();  
dataSet.ReadXmlSchema(xmlStream);  
xmlStream.Close();  
```  
  
## <a name="inferxmlschema"></a>InferXmlSchema  

 你还可以使用**dataset**的**InferXmlSchema**方法来指示**数据集**从 XML 文档推断其架构。 **InferXmlSchema** 的功能与 **ReadXml** 的 **XmlReadMode** 相同， **它 (加载** 数据，并推导架构) ，如果读取的文档不包含内联架构，则为 **ReadXmlSchema** 。 但是， **InferXmlSchema** 提供了额外的功能，使您可以指定在推断架构时要忽略的特定 XML 命名空间。 **InferXmlSchema** 采用两个必需的参数： XML 文档的位置，由文件名、流或 **XmlReader**指定;和要由操作忽略的 XML 命名空间的字符串数组。  
  
 例如，考虑以下 XML：  
  
```xml  
<NewDataSet xmlns:od="urn:schemas-microsoft-com:officedata">  
<Categories>  
  <CategoryID od:adotype="3">1</CategoryID>
  <CategoryName od:maxLength="15" od:adotype="130">Beverages</CategoryName>
  <Description od:adotype="203">Soft drinks and teas</Description>
</Categories>  
<Products>  
  <ProductID od:adotype="20">1</ProductID>
  <ReorderLevel od:adotype="3">10</ReorderLevel>
  <Discontinued od:adotype="11">0</Discontinued>
</Products>  
</NewDataSet>  
```  
  
 由于为前面的 XML 文档中的元素指定了属性， **ReadXmlSchema**方法和**XmlReadMode**为**InferSchema**的**ReadXml**方法将为文档中的每个元素创建表：**类别、类别**名称、**类别**、**说明**、**产品**、 **ProductID** **、** **ReorderLevel**和**废止**。  (有关详细信息，请参阅[从 XML 推断数据集关系结构](inferring-dataset-relational-structure-from-xml.md)。 ) 不过，更合适的结构是仅创建 "**类别**" 和 "**产品**" 表，然后在 "类别" 表中创建 "**类别** **id**"、"类别" 和 "**说明** **" 列**，在**Products**表中创建**ProductID**、 **ReorderLevel**和**废止**列。 若要确保推断的架构忽略 XML 元素中指定的属性，请使用 **InferXmlSchema** 方法并指定要忽略的 **officedata** 的 XML 命名空间，如以下示例中所示。  
  
```vb  
Dim dataSet As DataSet = New DataSet  
dataSet.InferXmlSchema("input_od.xml", New String() {"urn:schemas-microsoft-com:officedata"})  
```  
  
```csharp  
DataSet dataSet = new DataSet();  
dataSet.InferXmlSchema("input_od.xml", new string[] "urn:schemas-microsoft-com:officedata");  
```  
  
## <a name="see-also"></a>请参阅

- [在数据集中使用 XML](using-xml-in-a-dataset.md)
- [从 XML 架构派生数据集关系结构 (XSD)](deriving-dataset-relational-structure-from-xml-schema-xsd.md)
- [从 XML 推断数据集关系结构](inferring-dataset-relational-structure-from-xml.md)
- [从 XML 加载数据集](loading-a-dataset-from-xml.md)
- [数据集、数据表和数据视图](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
