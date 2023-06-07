---
title: 使用 XmlNameTable 的对象比较
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8d94e041-d340-4ddf-9a2c-d7319e3f4f86
ms.openlocfilehash: 5e0f5de6fd956bcaaf30b592e04c432a5f824c52
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734096"
---
# <a name="object-comparison-using-xmlnametable"></a>使用 XmlNameTable 的对象比较

创建后，XmlDocuments  会为相应文档专门创建名称表。 将 XML 加载到文档中或新建元素或属性后，属性名和元素名称就会被放入 XmlNameTable  中。 还可以使用另一个文档中的现有 NameTable  创建 XmlDocument  。 如果通过需要使用 XmlNameTable  参数的构造函数创建 XmlDocuments  ，文档有权访问已在 XmlNameTable  中存储的节点名称、命名空间和前缀。 无论如何为名称表加载名称，一旦名称存储在表中，便可以使用对象比较（而不是字符串比较）来快速比较名称。 也可以使用 <xref:System.Xml.NameTable.Add%2A> 将字符串添加到名称表中。 下面的代码示例展示了创建的名称表，以及添加到表中的字符串 MyString  。 完成上述操作之后，使用此表创建 XmlDocument  ，并将 Myfile.xml  中的元素名称和属性名添加到现有名称表中。  
  
```vb  
Dim nt As New NameTable()  
nt.Add("MyString")  
Dim doc As New XmlDocument(nt)  
doc.Load("Myfile.xml")  
```  
  
```csharp  
NameTable nt = new NameTable();  
nt.Add("MyString");  
XmlDocument doc = new XmlDocument(nt);  
doc.Load("Myfile.xml");  
```  
  
 下面的代码示例显示文档的创建，添加到该文档中的两个新元素（同时还将这两个元素添加到文档名称表中）以及针对名称进行的对象比较。  
  
```vb  
Dim doc1 As XmlDocument = imp.CreateDocument()  
Dim node1 As XmlElement = doc.CreateElement("node1")  
Dim doc2 As XmlDocument = imp.CreateDocument()  
Dim node2 As XmlElement = doc.CreateElement("node2")  
if (CType(node1.Name, object) = CType(node2.Name, object))  
```  
  
```csharp  
XmlDocument doc1 = imp.CreateDocument();  
node1 = doc1.CreateElement ("node1");  
XmlDocument doc2 = imp.CreateDocument();  
node2 = doc2.CreateElement ("node1");  
if (((object)node1.Name) == ((object)node2.Name))  
{ ...  
```  
  
 当反复处理同一类型的文档（如电子商务站点的订单文档，该文档符合 XML 架构定义语言 [即 XSD] 或文档类型定义 [即 DTD]）并重复相同的字符串时，通常使用以上在两个文档之间传递名称表的方案。 使用同一名称表可提高性能，因为同一元素名出现在多个文档中。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
