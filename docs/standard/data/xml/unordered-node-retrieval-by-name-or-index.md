---
title: 按名称或索引检索未排序节点
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 2038a90b-92af-4a0a-baaa-08e688d95194
ms.openlocfilehash: c3358f6cca27fbe61ba2b59fe6f2eb5df81c91ac
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95675641"
---
# <a name="unordered-node-retrieval-by-name-or-index"></a>按名称或索引检索未排序节点

XmlNamedNodeMap  在万维网联合会 (W3C) 规范中被描述为 NamedNodeMap，它对于处理未排序的节点集并能够按节点名称或索引引用节点是必需的。 访问 XmlNamedNodeMap  的唯一方式是通过方法或属性返回 XmlNamedNodeMap  。 有三个方法或属性返回 XmlNamedNodeMap  ：  
  
- XmlElement.Attributes  
  
- XmlDocumentType.Entities  
  
- XmlDocumentType.Notations  
  
 例如，XmlDocumentType.Entities  属性获取在文档类型声明中声明的 XmlEntity  节点集合。 此集合作为 XmlNamedNodeMap  返回，可以使用 Count  属性循环访问此集合并显示实体信息。 有关循环访问 XmlNamedNodeMap  的示例，请参见 <xref:System.Xml.XmlDocumentType.Entities%2A>。  
  
  XmlAttributeCollection 派生自 XmlNamedNodeMap  ，且只有属性是可以修改的，而表示法和实体则是只读的。 通过将 XmlNamedNodeMap  用于属性，可以基于属性的 XML 名称获取这些属性的节点。 这样，提供了一个处理元素节点的属性集合的简单方法。 该方法可与 XmlNodeList  直接进行比较，后者也实现 IEnumerable  接口但使用的是索引访问器而不是字符串。 RemoveNamedItem  和 SetNamedItem  方法只针对 XmlAttributeCollection  使用。 属性集合中的添加或移除操作（无论是使用 AttributeCollection  还是 XmlNamedNodeMap  实现）将修改元素的属性集合。 下面的代码示例显示如何移动属性并创建新属性。  
  
```vb  
Imports System  
Imports System.Xml  
  
Class test  
  
    Public Shared Sub Main()  
        Dim doc As New XmlDocument()  
        doc.LoadXml("<root> <child1 attr1='val1' attr2='val2'> text1 </child1> <child2 attr3='val3'> text2 </child2> </root> ")  
  
        ' Get the attributes of node "child2 "  
        Dim ac As XmlAttributeCollection = doc.DocumentElement.ChildNodes(1).Attributes  
  
        ' Print out the number of attributes and their names.  
        Console.WriteLine(("Number of Attributes: " + ac.Count))  
        Dim i As Integer  
        For i = 0 To ac.Count - 1  
            Console.WriteLine((i + 1 + ".  Attribute Name: '" + ac(i).Name + "'  Attribute Value:  '" + ac(i).Value + "'"))  
        Next i  
        ' Get the 'attr1' from child1.  
        Dim attr As XmlAttribute = doc.DocumentElement.ChildNodes(0).Attributes(0)  
  
        ' Add this attribute to the attributecollection "ac".  
        ac.SetNamedItem(attr)  
  
        ''attr1' will be removed from 'child1' and added to 'child2'.  
        ' Print out the number of attributes and their names.  
        Console.WriteLine(("Number of Attributes: " + ac.Count))  
  
        For i = 0 To ac.Count - 1  
            Console.WriteLine((i + 1 + ".  Attribute Name: '" + ac(i).Name + "'  Attribute Value:  '" + ac(i).Value + "'"))  
        Next i  
        ' Create a new attribute and add to the collection.  
        Dim attr2 As XmlAttribute = doc.CreateAttribute("attr4")  
        attr2.Value = "val4"  
        ac.SetNamedItem(attr2)  
  
        ' Print out the number of attributes and their names.  
        Console.WriteLine(("Number of Attributes: " + ac.Count))  
  
        For i = 0 To ac.Count - 1  
            Console.WriteLine((i + 1 + ".  Attribute Name: '" + ac(i).Name + "'  Attribute Value:  '" + ac(i).Value + "'"))  
        Next i  
    End Sub 'Main  
End Class 'test  
```  
  
```csharp  
using System;  
using System.Xml;  
class test {  
    public static void Main() {  
        XmlDocument doc = new XmlDocument();  
        doc.LoadXml( "<root> <child1 attr1='val1' attr2='val2'> text1 </child1> <child2 attr3='val3'> text2 </child2> </root> " );  
  
        // Get the attributes of node "child2"  
        XmlAttributeCollection ac = doc.DocumentElement.ChildNodes[1].Attributes;  
  
        // Print out the number of attributes and their names.  
        Console.WriteLine( "Number of Attributes: "+ac.Count );  
        for( int i = 0; i < ac.Count; i++ )  
            Console.WriteLine( (i+1) + ".  Attribute Name: '" +ac[i].Name+ "'  Attribute Value:  '"+ ac[i].Value +"'" );
  
        // Get the 'attr1' from child1.  
        XmlAttribute attr = doc.DocumentElement.ChildNodes[0].Attributes[0];  
  
        // Add this attribute to the attributecollection "ac".  
        ac.SetNamedItem( attr );  
  
        // 'attr1' will be removed from 'child1' and added to 'child2'.  
        // Print out the number of attributes and their names.  
        Console.WriteLine( "Number of Attributes: "+ac.Count );
        for( int i = 0; i < ac.Count; i++ )  
            Console.WriteLine( (i+1) + ".  Attribute Name: '" +ac[i].Name+ "'  Attribute Value:  '"+ ac[i].Value +"'" );
  
        // Create a new attribute and add to the collection.  
        XmlAttribute attr2 = doc.CreateAttribute( "attr4" );  
        attr2.Value = "val4";  
        ac.SetNamedItem( attr2 );  
  
        // Print out the number of attributes and their names.  
        Console.WriteLine( "Number of Attributes: "+ac.Count );
        for( int i = 0; i < ac.Count; i++ )  
            Console.WriteLine( (i+1) + ".  Attribute Name: '" +ac[i].Name+ "'  Attribute Value:  '"+ ac[i].Value +"'" );
  
    }  
}  
```  
  
 若要查看另一个显示从 AttributeCollection  中移除属性的代码示例，请参见 [XmlNamedNodeMap.RemoveNamedItem 方法](xref:System.Xml.XmlNamedNodeMap.RemoveNamedItem%2A)。 有关方法和属性的更多信息，请参见 [XmlNamedNodeMap 成员](xref:System.Xml.XmlNamedNodeMap)。  
  
## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
