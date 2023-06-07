---
title: 数据集和 XmlDataDocument 同步
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 0ce3793d-54b2-47e4-8cf7-b0591cc4dd21
ms.openlocfilehash: 2790f0a9edd5bfde96683e00725dd04555379adf
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153295"
---
# <a name="dataset-and-xmldatadocument-synchronization"></a>数据集和 XmlDataDocument 同步

ADO.NET <xref:System.Data.DataSet> 为您提供了数据的关系表示形式。 若要实现分层数据访问，可以使用 .NET Framework 中的可用 XML 类。 以前，数据的这两种表示形式是单独使用的。 不过，.NET Framework 通过分别通过 **DataSet** 对象和对象实现对数据的关系和分层表示形式的实时同步访问 <xref:System.Xml.XmlDataDocument> 。  
  
 如果 **数据集** 与 **XmlDataDocument**同步，则这两个对象使用单个数据集。 这意味着，如果对 **数据集**进行了更改，则所做的更改将反映在 **XmlDataDocument**中，反之亦然。 **数据集**和**XmlDataDocument**之间的关系通过允许单个应用程序、使用单个数据集，访问围绕**数据集**构建的整个服务套件 (例如 Web 窗体和 Windows 窗体控件以及 Visual Studio .NET 设计器) ，以及 Xml 服务套件，包括可扩展样式表语言 (XSL) 、XSL 转换 (XSLT) 和 XML 路径语言 (XPath) 。 您不必选择使应用程序以哪一组服务为目标，这两组服务都可用。  
  
 可以通过多种方式将 **数据集** 与 **XmlDataDocument**同步。 您可以：  
  
- 使用架构 (（即关系结构) 和数据）填充 **数据集** ，然后将其与新的 **XmlDataDocument**同步。 这将提供现有关系数据的分层视图。 例如：  
  
    ```vb  
    Dim dataSet As DataSet = New DataSet  
  
    ' Add code here to populate the DataSet with schema and data.  
  
    Dim xmlDoc As XmlDataDocument = New XmlDataDocument(dataSet)  
    ```  
  
    ```csharp  
    DataSet dataSet = new DataSet();  
  
    // Add code here to populate the DataSet with schema and data.  
  
    XmlDataDocument xmlDoc = new XmlDataDocument(dataSet);  
    ```  
  
- 使用仅限架构 (（如强类型化**数据集**) ）填充**数据集**，并将其与**XMLDATADOCUMENT**同步，然后从 XML 文档加载**XmlDataDocument** 。 这将提供现有分层数据的关系视图。 **数据集**架构中的表名和列名必须与要与其同步的 XML 元素的名称相匹配。 该匹配区分大小写。  
  
     请注意，该 **数据集** 的架构只需要匹配您要在关系视图中公开的 XML 元素。 这样，就可以具有非常大的 XML 文档，而该文档上可以有非常小的关系“窗口”。 即使**数据集**只公开其中的一小部分， **XmlDataDocument**仍保留整个 XML 文档。  (有关此情况的详细示例，请参阅 [使用 XmlDataDocument 同步数据集](synchronizing-a-dataset-with-an-xmldatadocument.md)。 )   
  
     下面的代码示例演示了创建 **数据集** 并填充其架构，然后将其与 **XmlDataDocument**同步的步骤。 请注意， **DataSet** 架构只需要与 **XmlDataDocument** 中要使用 **数据集**公开的元素匹配。  
  
    ```vb  
    Dim dataSet As DataSet = New DataSet  
  
    ' Add code here to populate the DataSet with schema, but not data.  
  
    Dim xmlDoc As XmlDataDocument = New XmlDataDocument(dataSet)  
    xmlDoc.Load("XMLDocument.xml")  
    ```  
  
    ```csharp  
    DataSet dataSet = new DataSet();  
  
    // Add code here to populate the DataSet with schema, but not data.  
  
    XmlDataDocument xmlDoc = new XmlDataDocument(dataSet);  
    xmlDoc.Load("XMLDocument.xml");  
    ```  
  
     如果 **XmlDataDocument** 与包含数据的 **数据集** 同步，则无法加载它。 否则会引发异常。  
  
- 创建新的**XmlDataDocument**并从 XML 文档加载它，然后使用**XmlDataDocument**的**DataSet**属性访问数据的关系视图。 你需要设置**数据集**的架构，然后才能使用**dataset**查看**XmlDataDocument**中的任何数据。 同样， **数据集** 架构中的表名和列名必须与要与其同步的 XML 元素的名称相匹配。 该匹配区分大小写。  
  
     下面的代码示例演示如何访问 **XmlDataDocument**中数据的关系视图。  
  
    ```vb  
    Dim xmlDoc As XmlDataDocument = New XmlDataDocument  
    Dim dataSet As DataSet = xmlDoc.DataSet  
  
    ' Add code here to create the schema of the DataSet to view the data.  
  
    xmlDoc.Load("XMLDocument.xml")  
    ```  
  
    ```csharp  
    XmlDataDocument xmlDoc = new XmlDataDocument();  
    DataSet dataSet = xmlDoc.DataSet;  
  
    // Add code here to create the schema of the DataSet to view the data.  
  
    xmlDoc.Load("XMLDocument.xml");  
    ```  
  
 同步 **XmlDataDocument** 与 **数据集** 的另一个优点是，XML 文档的保真度得以保留。 如果使用**ReadXml**从 xml 文档填充数据**集**，则当使用**WRITEXML**将数据写回 xml 文档时，它可能会与原始 XML 文档有很大差异。 这是因为 **数据集** 不会保留 XML 文档中的格式设置，如空白或层次结构信息（例如元素顺序）。 **数据集**还不包含 XML 文档中的元素，因为这些元素与**DataSet**的架构不匹配。 将 **XmlDataDocument** 与 **数据集** 同步，可以在 **XmlDataDocument**中维护原始 XML 文档的格式设置和分层元素结构，而 **数据集** 只包含适用于该数据 **集**的数据和架构信息。  
  
 将 **数据集** 与 **XmlDataDocument**同步时，结果可能会有所不同，具体取决于是否嵌套了您的 <xref:System.Data.DataRelation> 对象。 有关详细信息，请参阅 [嵌套 datarelation](nesting-datarelations.md)。  
  
## <a name="in-this-section"></a>本节内容  

 [将数据集和 XmlDataDocument 同步](synchronizing-a-dataset-with-an-xmldatadocument.md)  
 演示如何使用**XmlDataDocument**将强类型化**数据集**与最小架构同步。  
  
 [对数据集执行 XPath 查询](performing-an-xpath-query-on-a-dataset.md)  
 演示如何对 **数据集**的内容执行 XPath 查询。  
  
 [将 XSLT 转换应用于 DataSet](applying-an-xslt-transform-to-a-dataset.md)  
 演示如何将 XSLT 转换应用于 **数据集**的内容。  
  
## <a name="related-sections"></a>相关章节  

 [在数据集中使用 XML](using-xml-in-a-dataset.md)  
 介绍如何将数据 **集** 与 xml 作为数据源进行交互，包括将 **数据集** 的内容作为 XML 数据进行加载和保存。  
  
 [嵌套 DataRelation](nesting-datarelations.md)  
 讨论嵌套的 **DataRelation** 对象在以 XML 数据形式表示 **数据集** 的内容时的重要性，并描述如何创建这些关系。  
  
 [数据集、数据表和数据视图](index.md)  
 介绍 **数据集** ，以及如何使用它来管理应用程序数据以及与包括关系数据库和 XML 在内的数据源进行交互。  
  
 <xref:System.Xml.XmlDataDocument>  
 包含有关 **XmlDataDocument** 类的参考信息。  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 概述](../ado-net-overview.md)
