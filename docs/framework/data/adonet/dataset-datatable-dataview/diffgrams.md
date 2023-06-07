---
title: DiffGrams
ms.date: 03/30/2017
ms.assetid: 037f3991-7bbc-424b-b52e-8b03585d3e34
ms.openlocfilehash: aff9c2347fab51d853e19bd9dc16666c4ed549b5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172796"
---
# <a name="diffgrams"></a>DiffGrams

DiffGram 是用于标识数据元素的当前和原始版本的 XML 格式。 <xref:System.Data.DataSet> 使用 DiffGram 格式来加载和保持其内容，并将其内容序列化，以便通过网络连接来进行传输。 当以 <xref:System.Data.DataSet> DiffGram 形式编写时，它将使用所有必需的信息来准确地重新创建内容（尽管不是的架构），并包含 <xref:System.Data.DataSet> 来自 **原始** 行版本和 **当前** 行版本、行错误信息和行顺序的列值。  
  
 当从 XML Web services 发送和检索 <xref:System.Data.DataSet> 时，将隐式地使用 DiffGram 格式。 此外，当 <xref:System.Data.DataSet> 使用 **ReadXml** 方法从 xml 加载的内容时，或 <xref:System.Data.DataSet> 使用 **WriteXml** 方法在 xml 中编写的内容时，可以指定以 DiffGram 读取或写入内容。 有关详细信息，请参阅 [从 Xml 加载数据集](loading-a-dataset-from-xml.md) 和 [以 Xml 数据形式编写数据集内容](writing-dataset-contents-as-xml-data.md)。  
  
 虽然 DiffGram 格式主要由 .NET Framework 用作 <xref:System.Data.DataSet> 内容的序列化格式，但也可以使用 DiffGram 来修改 Microsoft SQL Server 数据库中表的数据。  
  
 通过将所有表的内容写入元素来生成 Diffgram **\<diffgram>** 。  
  
### <a name="to-generate-a-diffgram"></a>生成 Diffgram  
  
1. 生成根表（即没有任何父级的表）的列表。  
  
2. 对于列表中每个表及其子代，在 Diffgram 的第一部分中写出所有行的当前版本。  
  
3. 对于中的每个表 <xref:System.Data.DataSet> ，在 Diffgram 的部分中写出所有行的原始版本（如果有） **\<before>** 。  
  
4. 对于有错误的行，在 Diffgram 的部分中写入错误内容 **\<errors>** 。  
  
 将按照从 XML 文件的开头到结尾的顺序处理 Diffgram。  
  
### <a name="to-process-a-diffgram"></a>处理 Diffgram  
  
1. 处理包含行当前版本的 Diffgram 的第一部分。  
  
2. 处理 **\<before>** 包含已修改行和已删除行的原始行版本的第二个或部分。  
  
    > [!NOTE]
    > 如果某行标记为已删除，则删除操作还可删除该行的子代，具体取决于当前 `Cascade` 的 <xref:System.Data.DataSet> 属性。  
  
3. 处理 **\<errors>** 部分。 为本部分中各项的指定行和列设置错误信息。  
  
> [!NOTE]
> 如果将 <xref:System.Data.XmlWriteMode> 设置为 Diffgram，则目标 <xref:System.Data.DataSet> 和原始 <xref:System.Data.DataSet> 的内容可能会不同。  
  
## <a name="diffgram-format"></a>DiffGram 格式  

 DiffGram 格式可分为三部分：当前数据、原始（或“before”）数据和错误部分，如下面的示例中所示。  
  
```xml  
<?xml version="1.0"?>  
<diffgr:diffgram
         xmlns:msdata="urn:schemas-microsoft-com:xml-msdata"  
         xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1"  
         xmlns:xsd="http://www.w3.org/2001/XMLSchema">  
  
   <DataInstance>  
   </DataInstance>  
  
  <diffgr:before>  
  </diffgr:before>  
  
  <diffgr:errors>  
  </diffgr:errors>  
</diffgr:diffgram>  
```  
  
 DiffGram 格式由以下数据块组成：  
  
 **\<**  ***DataInstance***  **>**  
 此元素的名称 ***DataInstance***用于此文档中的说明。 ***DataInstance***元素表示或的 <xref:System.Data.DataSet> 行 <xref:System.Data.DataTable> 。 元素将包含或的名称，而不是 *DataInstance* <xref:System.Data.DataSet> <xref:System.Data.DataTable> 。 此 DiffGram 格式块包含当前数据（无论是否经过修改）。 使用 **diffgr： hasChanges** 批注标识已修改的元素或行。  
  
 **\<diffgr:before>**  
 此 DiffGram 格式块包含行的原始版本。 此块中的元素与使用**diffgr： id**批注的***DataInstance***块中的元素匹配。  
  
 **\<diffgr:errors>**  
 此 DiffGram 格式块包含 ***DataInstance*** 块中特定行的错误信息。 此块中的元素与使用**diffgr： id**批注的***DataInstance***块中的元素匹配。  
  
## <a name="diffgram-annotations"></a>DiffGram 批注  

 DiffGram 使用一些批注来使来自不同 DiffGram 块的元素相关，这些块表示 <xref:System.Data.DataSet> 中的不同行版本或错误信息。  
  
 下表描述了在 DiffGram 命名空间 urn 中定义的 DiffGram 注释 **：架构-microsoft-com： xml-DiffGram-v1**。  
  
|Annotation|说明|  
|----------------|-----------------|  
|**id**|用于将和中的元素与 **\<diffgr:before>** **\<diffgr:errors>** 块中的元素配对 **\<** ***DataInstance*** **>** 。 带有 **diffgr： id** 批注的值的格式为 *[TableName] [RowIdentifier]*。 例如： `<Customers diffgr:id="Customers1">`。|  
|**parentId**|标识块中的哪个元素 **\<** ***DataInstance*** **>** 是当前元素的父元素。 具有 **diffgr： parentId** 批注的值的格式为 *[TableName] [RowIdentifier]*。 例如： `<Orders diffgr:parentId="Customers1">`。|  
|**hasChanges**|将块中的行标识 **\<** ***DataInstance*** **>** 为已修改。 **HasChanges**批注可以具有以下两个值之一：<br /><br /> **放**<br /> 标识 **已添加** 的行。<br /><br /> **已修改**<br /> 标识在块中包含**原始**行版本的**已修改**行 **\<diffgr:before>** 。 请注意， **已删除** 的行在块中将具有 **原始** 行版本 **\<diffgr:before>** ，但在块中将不存在批注元素 **\<** ***DataInstance*** **>** 。|  
|**hasErrors**|使用 RowError 标识块中的行 **\<** ***DataInstance*** **>** 。 **RowError** 错误元素放置在 **\<diffgr:errors>** 块中。|  
|**错误**|包含块中特定元素的 **RowError** 文本 **\<diffgr:errors>** 。|  
  
 当以 DiffGram 格式读写 <xref:System.Data.DataSet> 的内容时，还包含附加的批注。 下表描述了在命名空间 **urn：架构-microsoft-msdata**中定义的这些附加批注。  
  
|Annotation|描述|  
|----------------|-----------------|  
|**RowOrder**|保留原始数据的行顺序并标识特定 <xref:System.Data.DataTable> 中行的索引。|  
|**Hidden**|将 **ColumnMapping** 属性设置为 mappingtype.attribute 的列标识为 **隐藏**。 该属性以 **msdata： hidden** *[ColumnName]*= "*value*" 格式编写。 例如： `<Customers diffgr:id="Customers1" msdata:hiddenContactTitle="Owner">`。<br /><br /> 请注意，只有当隐藏列包含数据时才以 DiffGram 属性的形式来编写隐藏列。 否则会忽视优先级。|  
  
## <a name="sample-diffgram"></a>DiffGram 示例  

 下面是 DiffGram 格式的示例。 该示例显示对表行的更新在提交更改之前的结果。 CustomerID 为“ALFKI”的行已被修改，但尚未更新。 因此，在块中有一个具有**diffgr： id** "Customers1" 的**当前**行 **\<** ***DataInstance*** **>** ，并在块中有一个具有**diffgr： id** "Customers1" 的**原始**行 **\<diffgr:before>** 。 CustomerID 为 "ANATR" 的行包含一个 **RowError**，因此它使用进行批注， `diffgr:hasErrors="true"` 且在块中有一个相关的元素 **\<diffgr:errors>** 。  
  
```xml  
<diffgr:diffgram xmlns:msdata="urn:schemas-microsoft-com:xml-msdata" xmlns:diffgr="urn:schemas-microsoft-com:xml-diffgram-v1">  
  <CustomerDataSet>  
    <Customers diffgr:id="Customers1" msdata:rowOrder="0" diffgr:hasChanges="modified">  
      <CustomerID>ALFKI</CustomerID>  
      <CompanyName>New Company</CompanyName>  
    </Customers>  
    <Customers diffgr:id="Customers2" msdata:rowOrder="1" diffgram:hasErrors="true">  
      <CustomerID>ANATR</CustomerID>  
      <CompanyName>Ana Trujillo Emparedados y Helados</CompanyName>  
    </Customers>  
    <Customers diffgr:id="Customers3" msdata:rowOrder="2">  
      <CustomerID>ANTON</CustomerID>  
      <CompanyName>Antonio Moreno Taquera</CompanyName>  
    </Customers>  
    <Customers diffgr:id="Customers4" msdata:rowOrder="3">  
      <CustomerID>AROUT</CustomerID>  
      <CompanyName>Around the Horn</CompanyName>  
    </Customers>  
  </CustomerDataSet>  
  <diffgr:before>  
    <Customers diffgr:id="Customers1" msdata:rowOrder="0">  
      <CustomerID>ALFKI</CustomerID>  
      <CompanyName>Alfreds Futterkiste</CompanyName>  
    </Customers>  
  </diffgr:before>  
  <diffgr:errors>  
    <Customers diffgr:id="Customers2" diffgr:Error="An optimistic concurrency violation has occurred for this row."/>  
  </diffgr:errors>  
</diffgr:diffgram>  
```  
  
## <a name="see-also"></a>请参阅

- [在数据集中使用 XML](using-xml-in-a-dataset.md)
- [从 XML 加载数据集](loading-a-dataset-from-xml.md)
- [写入数据集内容作为 XML 数据](writing-dataset-contents-as-xml-data.md)
- [数据集、数据表和数据视图](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
