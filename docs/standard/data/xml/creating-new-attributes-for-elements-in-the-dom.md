---
title: 为 DOM 中的元素创建新属性
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: dd6dc920-b011-418a-b3db-f1580a7d9251
ms.openlocfilehash: 1cb37e47bedf955ea2c6f9faad628df2175fb703
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94826022"
---
# <a name="creating-new-attributes-for-elements-in-the-dom"></a>为 DOM 中的元素创建新属性

新建属性不同于创建其他节点类型，因为属性不是节点，而是元素节点的属性，包含在与元素关联的 XmlAttributeCollection  中。 有多种方法可创建属性并将其附加到元素：

- 获取元素节点，并使用 SetAttribute  将属性添加到相应元素的属性集合。

- 使用 CreateAttribute  方法创建 XmlAttribute  节点，获取元素节点，再使用 SetAttributeNode  将节点添加到相应元素的属性集合。

下面的示例展示了如何使用 SetAttribute  方法将属性添加到元素：

```vb
Imports System.IO
Imports System.Xml

Public Class Sample

    Public Shared Sub Main()

        Dim doc As New XmlDocument()
        doc.LoadXml("<book xmlns:bk='urn:samples' bk:ISBN='1-861001-57-5'>" & _
                    "<title>Pride And Prejudice</title>" & _
                    "</book>")
        Dim root As XmlElement = doc.DocumentElement

        ' Add a new attribute.
        root.SetAttribute("genre", "urn:samples", "novel")

        Console.WriteLine("Display the modified XML...")
        Console.WriteLine(doc.InnerXml)
    End Sub
End Class
```  
  
```csharp
using System;
using System.IO;
using System.Xml;

public class Sample
{
    public static void Main()
    {
        var doc = new XmlDocument();
        doc.LoadXml("<book xmlns:bk='urn:samples' bk:ISBN='1-861001-57-5'>" +
                    "<title>Pride And Prejudice</title>" +
                    "</book>");
        XmlElement root = doc.DocumentElement;

        // Add a new attribute.
        root.SetAttribute("genre", "urn:samples", "novel");

        Console.WriteLine("Display the modified XML...");
        Console.WriteLine(doc.InnerXml);
    }
}
```

下面的示例展示了使用 CreateAttribute  方法新建的属性。 然后展示了使用 SetAttributeNode  方法添加到 book  元素的属性集合的属性。

已知下列 XML：
  
```xml
<book genre='novel' ISBN='1-861001-57-5'>
<title>Pride And Prejudice</title>
</book>
```

创建一个新属性并为其提供值：

```vb
Dim attr As XmlAttribute = doc.CreateAttribute("publisher")
attr.Value = "WorldWide Publishing"
```

```csharp
XmlAttribute attr = doc.CreateAttribute("publisher");
attr.Value = "WorldWide Publishing";
```

将其附加到此元素：

```vb
doc.DocumentElement.SetAttributeNode(attr)
```

```csharp
doc.DocumentElement.SetAttributeNode(attr);
```

**输出**

```xml
<book genre="novel" ISBN="1-861001-57-5" publisher="WorldWide Publishing">
<title>Pride And Prejudice</title>
</book>
```

有关完整代码示例，可以参阅 <xref:System.Xml.XmlDocument.CreateAttribute%2A>。

也可以创建 XmlAttribute  节点，并使用 InsertBefore  或 InsertAfter  方法，将它添加到集合中的适当位置。 如果属性集合中已有同名的属性，将会从集合中删除现有 XmlAttribute  节点，并插入新的 XmlAttribute  节点。 这与 SetAttribute  方法执行的操作一样。 这些方法需要使用现有节点作为参数，以用作执行 InsertBefore  和 InsertAfter  的参考点。 如果不提供指示新节点插入位置的参考节点，InsertAfter  方法默认在集合的开头插入新节点。 如果未提供任何参考节点，InsertBefore  默认在集合的末尾插入新节点。

如果创建了属性的 XmlNamedNodeMap  ，可以使用 <xref:System.Xml.XmlNamedNodeMap.SetNamedItem%2A> 方法按名称添加属性。 有关详细信息，请参阅 [NamedNodeMaps 和 NodeLists 中的节点集合](node-collections-in-namednodemaps-and-nodelists.md)。

## <a name="default-attributes"></a>默认属性

如果创建一个声明为具有默认属性的元素，则 XML 文档对象模型 (DOM) 创建一个带默认值的新默认属性并将其附加到该元素。 此时还创建默认属性的子节点。

## <a name="attribute-child-nodes"></a>属性子节点

属性节点的值成为它的子节点。 有效子节点只有两种类型：XmlText  节点和 XmlEntityReference  节点。 这些之所以是子节点是因为，FirstChild  和 LastChild  等方法按子节点处理它们。 当试图移除属性或属性子节点时，属性这种具有子节点的特性很重要。 有关详细信息，请参阅[删除 DOM 中元素节点的属性](removing-attributes-from-an-element-node-in-the-dom.md)。

## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
