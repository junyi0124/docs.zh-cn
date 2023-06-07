---
title: 用于编译架构的 XmlSchemaSet
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 55c4b175-3170-4071-9d60-dd5a42f79b54
ms.openlocfilehash: 4501f3dde8d402bd318332dfe9b2209b3febea71
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723358"
---
# <a name="xmlschemaset-for-schema-compilation"></a>用于编译架构的 XmlSchemaSet

介绍 <xref:System.Xml.Schema.XmlSchemaSet>，一个可以存储和验证 XML 架构定义语言 (XSD) 架构的缓存。  
  
## <a name="the-xmlschemaset-class"></a>XmlSchemaSet 类  

 <xref:System.Xml.Schema.XmlSchemaSet> 是一个可以存储和验证 XML 架构定义语言 (XSD) 架构的缓存。  
  
 在 <xref:System.Xml?displayProperty=nameWithType> 1.0 版中，XML 架构作为架构库加载到 <xref:System.Xml.Schema.XmlSchemaCollection> 类中。 在 <xref:System.Xml?displayProperty=nameWithType> 2.0 版中，<xref:System.Xml.XmlValidatingReader> 和 <xref:System.Xml.Schema.XmlSchemaCollection> 类已过时，分别由 <xref:System.Xml.XmlReader.Create%2A> 方法和 <xref:System.Xml.Schema.XmlSchemaSet> 类所取代。  
  
 引入了 <xref:System.Xml.Schema.XmlSchemaSet> 来解决许多问题，包括标准兼容性、性能和过时的 Microsoft XML 数据简化 (XDR) 架构格式。  
  
 以下是 <xref:System.Xml.Schema.XmlSchemaCollection> 类和 <xref:System.Xml.Schema.XmlSchemaSet> 类之间的比较。  
  
|XmlSchemaCollection|XmlSchemaSet|  
|-------------------------|------------------|  
|支持 Microsoft XDR 和 W3C XML 架构。|只支持 W3C XML 架构。|  
|架构在调用 <xref:System.Xml.Schema.XmlSchemaCollection.Add%2A> 方法时编译。|架构在调用 <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> 方法时不编译。 这样可以提高创建架构库时的性能。|  
|每个架构都会生成单独编译的版本，这就会产生各个“架构岛”。 因此，所有包含和导入仅在该架构中有效。|已编译的架构生成单个逻辑架构，即一个架构“集”。 添加到该架构集的架构内的任何导入架构将直接添加到架构集中。 这意味着所有类型可以供所有架构使用。|  
|特定目标命名空间在集合中只能存在一个架构。|只要不存在类型冲突，就可以为同一目标命名空间添加多个架构。|  
  
## <a name="migrating-to-the-xmlschemaset"></a>迁移到 XmlSchemaSet  

 以下代码示例提供从过时的 <xref:System.Xml.Schema.XmlSchemaSet> 类迁移到新的 <xref:System.Xml.Schema.XmlSchemaCollection> 类的指南。 该代码示例说明两个类之间的下列主要区别。  
  
- 与 <xref:System.Xml.Schema.XmlSchemaCollection.Add%2A> 类的 <xref:System.Xml.Schema.XmlSchemaCollection> 方法不同，在调用 <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> 的 <xref:System.Xml.Schema.XmlSchemaSet> 方法时，不编译架构。 <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> 的 <xref:System.Xml.Schema.XmlSchemaSet> 方法在示例代码中显式调用。  
  
- 要循环访问 <xref:System.Xml.Schema.XmlSchemaSet>，必须使用 <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> 的 <xref:System.Xml.Schema.XmlSchemaSet> 属性。  
  
 以下是过时的 <xref:System.Xml.Schema.XmlSchemaCollection> 代码示例。  
  
```vb  
Dim schemaCollection As XmlSchemaCollection = New XmlSchemaCollection()  
schemaCollection.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd")  
schemaCollection.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd")  
  
Dim schema As XmlSchema  
  
For Each schema in schemaCollection  
  
   Console.WriteLine(schema.TargetNamespace)  
  
Next  
```  
  
```csharp  
XmlSchemaCollection schemaCollection = new XmlSchemaCollection();  
schemaCollection.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd");  
schemaCollection.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd");  
  
foreach(XmlSchema schema in schemaCollection)  
{  
   Console.WriteLine(schema.TargetNamespace);  
}  
```  
  
 以下是等效的 <xref:System.Xml.Schema.XmlSchemaSet> 代码示例。  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet()  
schemaSet.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd")  
schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd")  
schemaSet.Compile()  
  
Dim schema As XmlSchema  
  
For Each schema in schemaSet.Schemas()  
  
   Console.WriteLine(schema.TargetNamespace)  
  
Next  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
schemaSet.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd");  
schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd");  
schemaSet.Compile();  
  
foreach(XmlSchema schema in schemaSet.Schemas())  
{  
   Console.WriteLine(schema.TargetNamespace);  
}  
```  
  
## <a name="adding-and-retrieving-schemas"></a>添加和检索架构  

 架构是使用 <xref:System.Xml.Schema.XmlSchemaSet> 的 <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> 方法添加到 <xref:System.Xml.Schema.XmlSchemaSet> 中的。 架构添加到 <xref:System.Xml.Schema.XmlSchemaSet> 中后，将与目标命名空间 URI 关联。 目标命名空间 URI 可以指定为 <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> 方法的参数，如果未指定目标命名空间，<xref:System.Xml.Schema.XmlSchemaSet> 将使用架构中定义的目标命名空间。  
  
 架构是使用 <xref:System.Xml.Schema.XmlSchemaSet> 的 <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> 属性从 <xref:System.Xml.Schema.XmlSchemaSet> 检索的。 通过 <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> 的 <xref:System.Xml.Schema.XmlSchemaSet> 属性，可以循环访问 <xref:System.Xml.Schema.XmlSchema> 中包含的 <xref:System.Xml.Schema.XmlSchemaSet> 对象。 <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> 属性返回 <xref:System.Xml.Schema.XmlSchema> 中包含的所有 <xref:System.Xml.Schema.XmlSchemaSet> 对象，如果给定了目标命名空间参数，则返回属于该目标命名空间的所有 <xref:System.Xml.Schema.XmlSchema> 对象。 如果 `null` 指定为目标命名空间参数，<xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> 属性将返回所有没有命名空间的架构。  
  
 以下示例将 `books.xsd` 命名空间中的 `http://www.contoso.com/books` 架构添加到 <xref:System.Xml.Schema.XmlSchemaSet>，从 `http://www.contoso.com/books` 检索所有属于 <xref:System.Xml.Schema.XmlSchemaSet> 命名空间的架构，然后将这些架构写入 <xref:System.Console>。  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet  
schemaSet.Add("http://www.contoso.com/books", "books.xsd")  
  
Dim schema As XmlSchema  
  
For Each schema In schemaSet.Schemas("http://www.contoso.com/books")  
  
   schema.Write(Console.Out)  
  
Next  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
schemaSet.Add("http://www.contoso.com/books", "books.xsd");  
  
foreach (XmlSchema schema in schemaSet.Schemas("http://www.contoso.com/books"))  
{  
   schema.Write(Console.Out);  
}  
```  
  
 有关在 <xref:System.Xml.Schema.XmlSchemaSet> 对象中添加和检索架构的更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> 方法和 <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A> 属性的参考文档。  
  
## <a name="compiling-schemas"></a>编译架构  

 <xref:System.Xml.Schema.XmlSchemaSet> 中的架构通过 <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> 的 <xref:System.Xml.Schema.XmlSchemaSet> 方法编译为一个逻辑架构。  
  
> [!NOTE]
> 与过时的 <xref:System.Xml.Schema.XmlSchemaCollection> 类不同，架构在调用 <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> 方法时不编译。  
  
 如果 <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> 方法成功执行，<xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> 的 <xref:System.Xml.Schema.XmlSchemaSet> 属性将设置为 `true`。  
  
> [!NOTE]
> 如果架构在 <xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> 中编辑，<xref:System.Xml.Schema.XmlSchemaSet> 属性不受影响。 不跟踪对 <xref:System.Xml.Schema.XmlSchemaSet> 中各个架构的更新。 因此，只要 <xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> 中没有添加或移除任何架构，即使 `true` 中包含的一个架构已更改，<xref:System.Xml.Schema.XmlSchemaSet> 属性也可以为 <xref:System.Xml.Schema.XmlSchemaSet>。  
  
 以下示例将 `books.xsd` 文件添加到 <xref:System.Xml.Schema.XmlSchemaSet> 中，然后调用 <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> 方法。  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet()  
schemaSet.Add("http://www.contoso.com/books", "books.xsd")  
schemaSet.Compile()  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
schemaSet.Add("http://www.contoso.com/books", "books.xsd");  
schemaSet.Compile();  
```  
  
 有关编译 <xref:System.Xml.Schema.XmlSchemaSet> 中的架构的更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> 方法参考文档。  
  
## <a name="reprocessing-schemas"></a>重新处理架构  

 重新处理 <xref:System.Xml.Schema.XmlSchemaSet> 中的架构时，将执行调用 <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> 的 <xref:System.Xml.Schema.XmlSchemaSet> 方法时对架构执行的所有预处理步骤。 如果调用 <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> 方法成功，<xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> 的 <xref:System.Xml.Schema.XmlSchemaSet> 属性将设置为 `false`。  
  
 如果 <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> 中的架构在 <xref:System.Xml.Schema.XmlSchemaSet> 执行编译之后已修改，应使用 <xref:System.Xml.Schema.XmlSchemaSet> 方法。  
  
 以下示例说明如何使用 <xref:System.Xml.Schema.XmlSchemaSet> 方法重新处理添加到 <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> 中的架构。 在使用 <xref:System.Xml.Schema.XmlSchemaSet> 方法编译了 <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A> 后，并且修改了添加到 <xref:System.Xml.Schema.XmlSchemaSet> 中的架构，即使 <xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> 中的架构已修改，`true` 属性仍将设置为 <xref:System.Xml.Schema.XmlSchemaSet>。 调用 <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> 方法将执行通过 <xref:System.Xml.Schema.XmlSchemaSet.Add%2A> 方法执行的所有预处理步骤，并将 <xref:System.Xml.Schema.XmlSchemaSet.IsCompiled%2A> 属性设置为 `false`。  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet()  
Dim schema As XmlSchema = schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd")  
schemaSet.Compile()  
  
Dim element As XmlSchemaElement = New XmlSchemaElement()  
schema.Items.Add(element)  
element.Name = "book"  
element.SchemaTypeName = New XmlQualifiedName("string", "http://www.w3.org/2001/XMLSchema")  
  
schemaSet.Reprocess(schema)  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
XmlSchema schema = schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd");  
schemaSet.Compile();  
  
XmlSchemaElement element = new XmlSchemaElement();  
schema.Items.Add(element);  
element.Name = "book";  
element.SchemaTypeName = new XmlQualifiedName("string", "http://www.w3.org/2001/XMLSchema");  
  
schemaSet.Reprocess(schema);  
```  
  
 有关重新处理 <xref:System.Xml.Schema.XmlSchemaSet> 中的架构的更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A> 方法参考文档。  
  
## <a name="checking-for-a-schema"></a>检查架构  

 可以使用 <xref:System.Xml.Schema.XmlSchemaSet.Contains%2A> 的 <xref:System.Xml.Schema.XmlSchemaSet> 方法检查架构是否包含在 <xref:System.Xml.Schema.XmlSchemaSet> 中。 <xref:System.Xml.Schema.XmlSchemaSet.Contains%2A> 方法使用目标命名空间或要检查的 <xref:System.Xml.Schema.XmlSchema> 对象。 在任何一种情况下，如果架构包含在 <xref:System.Xml.Schema.XmlSchemaSet.Contains%2A> 中，`true` 方法将返回 <xref:System.Xml.Schema.XmlSchemaSet>；否则，将返回 `false`。  
  
 有关检查架构的更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaSet.Contains%2A> 方法参考文档。  
  
## <a name="removing-schemas"></a>移除架构  

 架构使用 <xref:System.Xml.Schema.XmlSchemaSet> 的 <xref:System.Xml.Schema.XmlSchemaSet.Remove%2A> 和 <xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A> 方法从 <xref:System.Xml.Schema.XmlSchemaSet> 中移除。 <xref:System.Xml.Schema.XmlSchemaSet.Remove%2A> 方法将指定架构从 <xref:System.Xml.Schema.XmlSchemaSet> 中移除，而 <xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A> 方法将指定架构及其导入的所有架构从 <xref:System.Xml.Schema.XmlSchemaSet> 中移除。  
  
 以下示例说明如何将多个架构添加到 <xref:System.Xml.Schema.XmlSchemaSet> 中，然后使用 <xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A> 方法移除其中一个架构及其导入的所有架构。  
  
```vb  
Dim schemaSet As XmlSchemaSet = New XmlSchemaSet()  
schemaSet.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd")  
schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd")  
schemaSet.Add("http://www.contoso.com/music", "http://www.contoso.com/music.xsd")  
  
Dim schema As XmlSchema  
  
For Each schema In schemaSet.Schemas()  
  
   If schema.TargetNamespace = "http://www.contoso.com/music" Then  
      schemaSet.RemoveRecursive(schema)  
   End If  
  
Next  
```  
  
```csharp  
XmlSchemaSet schemaSet = new XmlSchemaSet();  
schemaSet.Add("http://www.contoso.com/retail", "http://www.contoso.com/retail.xsd");  
schemaSet.Add("http://www.contoso.com/books", "http://www.contoso.com/books.xsd");  
schemaSet.Add("http://www.contoso.com/music", "http://www.contoso.com/music.xsd");  
  
foreach (XmlSchema schema in schemaSet.Schemas())  
{  
   if (schema.TargetNamespace == "http://www.contoso.com/music")  
   {  
      schemaSet.RemoveRecursive(schema);  
   }  
}  
```  
  
 有关移除 <xref:System.Xml.Schema.XmlSchemaSet> 中的架构的更多信息，请参见 <xref:System.Xml.Schema.XmlSchemaSet.Remove%2A> 和 <xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A> 方法的参考文档。  
  
## <a name="schema-resolution-and-xsimport"></a>架构解析和 xs:import  

 下列示例说明 <xref:System.Xml.Schema.XmlSchemaSet> 中存在给定命名空间的多个架构时，用于导入架构的 <xref:System.Xml.Schema.XmlSchemaSet> 行为。  
  
 例如，考虑包含 <xref:System.Xml.Schema.XmlSchemaSet> 命名空间的多个架构的 `http://www.contoso.com`。 具有以下 `xs:import` 指令的架构添加到 <xref:System.Xml.Schema.XmlSchemaSet> 中。  
  
```xml  
<xs:import namespace="http://www.contoso.com" schemaLocation="http://www.contoso.com/schema.xsd" />  
```  
  
 <xref:System.Xml.Schema.XmlSchemaSet> 尝试导入 `http://www.contoso.com` 命名空间的架构，方法是从 `http://www.contoso.com/schema.xsd` URL 加载该架构。 只有架构文档中声明的架构声明和类型可以在导入架构中使用，即使 `http://www.contoso.com` 中存在 <xref:System.Xml.Schema.XmlSchemaSet> 命名空间的其他架构文档。 如果 `schema.xsd` 文件不能位于 `http://www.contoso.com/schema.xsd` URL，则不会有任何 `http://www.contoso.com` 命名空间的架构导入到导入架构中。  
  
## <a name="validating-xml-documents"></a>验证 XML 文档  

 XML 文档可以针对 <xref:System.Xml.Schema.XmlSchemaSet> 中的架构进行验证。 若要验证 XML 文档，可以将架构添加到 <xref:System.Xml.XmlReaderSettings> 对象的 <xref:System.Xml.Schema.XmlSchemaSet><xref:System.Xml.XmlReaderSettings.Schemas%2A> 属性中，也可以将 <xref:System.Xml.Schema.XmlSchemaSet> 添加到 <xref:System.Xml.XmlReaderSettings> 对象的 <xref:System.Xml.XmlReaderSettings.Schemas%2A> 属性中。 然后，<xref:System.Xml.XmlReaderSettings> 类的 <xref:System.Xml.XmlReader.Create%2A> 方法使用 <xref:System.Xml.XmlReader> 对象创建一个 <xref:System.Xml.XmlReader> 对象并验证该 XML 文档。  
  
 若要详细了解如何使用 <xref:System.Xml.Schema.XmlSchemaSet> 验证 XML 文档，请参阅[使用 XmlSchemaSet 进行 XML 架构 (XSD) 验证](xml-schema-xsd-validation-with-xmlschemaset.md)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.Schema.XmlSchemaSet.Add%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Schemas%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Contains%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Compile%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Reprocess%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.Remove%2A>
- <xref:System.Xml.Schema.XmlSchemaSet.RemoveRecursive%2A>
- [XmlSchemaSet 作为架构缓存](xmlschemaset-for-schema-compilation.md)
- [使用 XmlSchemaSet 进行 XML 架构 (XSD) 验证](xml-schema-xsd-validation-with-xmlschemaset.md)
