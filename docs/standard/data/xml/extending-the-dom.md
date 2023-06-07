---
title: 扩展 DOM
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: b5489c96-4afd-439a-a25d-fc82eb4a148d
ms.openlocfilehash: 173d36f534f951cfafefd3bd69b7ab245d575747
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829500"
---
# <a name="extending-the-dom"></a>扩展 DOM

Microsoft .NET Framework 包含一组基类，用于实现 XML 文档对象模型 (DOM)。 <xref:System.Xml.XmlNode> 及其派生类提供的方法和属性可以浏览、查询和修改 XML 文档的内容和结构。

当使用 DOM 将 XML 内容加载到内存中时，所创建的节点包含节点名、节点类型等信息。 可能存在需要特定节点信息但基类未提供的情况。 例如，可能需要查看节点的行号和位置。 这种情况下，可以从现有的 DOM 类派生新类并添加附加功能。

派生新类时有两个一般原则：

- 建议永远不要从 <xref:System.Xml.XmlNode> 类派生。 相反，建议从与对您有用的节点类型对应的类派生类。 例如，如果要返回属性节点的其他信息，可从 <xref:System.Xml.XmlAttribute> 类派生。

- 除了节点创建方法外，建议在重写函数时总是调用基本版本的函数，然后添加任何附加处理。

## <a name="creating-your-own-node-instances"></a>创建您自己的节点实例

<xref:System.Xml.XmlDocument> 类包含节点创建方法。 加载 XML 文件时，调用这些方法来创建节点。 可以重写这些方法，以便在加载文档时创建节点实例。 例如，如果已经扩展了 <xref:System.Xml.XmlElement> 类，则可以继承 <xref:System.Xml.XmlDocument> 类并重写 <xref:System.Xml.XmlDocument.CreateElement%2A> 方法。

下面的示例显示如何重写 <xref:System.Xml.XmlDocument.CreateElement%2A> 方法以返回 <xref:System.Xml.XmlElement> 类的实现。

```vb
Class LineInfoDocument
    Inherits XmlDocument
        Public Overrides Function CreateElement(prefix As String, localname As String, nsURI As String) As XmlElement
        Dim elem As New LineInfoElement(prefix, localname, nsURI, Me)
        Return elem
    End Function 'CreateElement
End Class 'LineInfoDocument
```

```csharp
class LineInfoDocument : XmlDocument
{
    public override XmlElement CreateElement(string prefix, string localname, string nsURI)
    {
        LineInfoElement elem = new LineInfoElement(prefix, localname, nsURI, this);
        return elem;
    }
}
```

## <a name="extending-a-class"></a>扩展类

若要扩展类，请从现有 DOM 类之一派生类。 然后，可以重写基类中的任何虚拟方法或属性或添加您自己的虚拟方法或属性。

在下面的示例中，创建了一个新类，该类实现 <xref:System.Xml.XmlElement> 类和 <xref:System.Xml.IXmlLineInfo> 接口。 定义了允许用户收集行信息的附加方法和属性。

```vb
Class LineInfoElement
   Inherits XmlElement
   Implements IXmlLineInfo
   Private lineNumber As Integer = 0
   Private linePosition As Integer = 0

   Friend Sub New(prefix As String, localname As String, nsURI As String, doc As XmlDocument)
      MyBase.New(prefix, localname, nsURI, doc)
      CType(doc, LineInfoDocument).IncrementElementCount()
   End Sub

   Public Sub SetLineInfo(linenum As Integer, linepos As Integer)
      lineNumber = linenum
      linePosition = linepos
   End Sub

   Public ReadOnly Property LineNumber() As Integer
      Get
         Return lineNumber
      End Get
   End Property

   Public ReadOnly Property LinePosition() As Integer
      Get
         Return linePosition
      End Get
   End Property

   Public Function HasLineInfo() As Boolean
      Return True
   End Function
End Class ' End LineInfoElement class.
```

```csharp
class LineInfoElement : XmlElement, IXmlLineInfo {
   int lineNumber = 0;
   int linePosition = 0;
   internal LineInfoElement( string prefix, string localname, string nsURI, XmlDocument doc ) : base( prefix, localname, nsURI, doc ) {
       ( (LineInfoDocument)doc ).IncrementElementCount();
  }
  public void SetLineInfo( int linenum, int linepos ) {
      lineNumber = linenum;
      linePosition = linepos;
  }
  public int LineNumber {
     get {
       return lineNumber;
     }
  }
  public int LinePosition {
      get {
        return linePosition;
      }
  }
  public bool HasLineInfo() {
    return true;
  }
} // End LineInfoElement class.
```

### <a name="example"></a>示例

以下示例计算 XML 文档中的元素数：

```vb
Imports System.Xml
Imports System.IO

Class LineInfoDocument
   Inherits XmlDocument

   Private elementCount As Integer

   Friend Sub New()
      elementCount = 0
   End Sub

   Public Overrides Function CreateElement(prefix As String, localname As String, nsURI As String) As XmlElement
      Dim elem As New LineInfoElement(prefix, localname, nsURI, Me)
      Return elem
   End Function

   Public Sub IncrementElementCount()
      elementCount += 1
   End Sub

   Public Function GetCount() As Integer
      Return elementCount
   End Function
End Class 'End LineInfoDocument class.

Class LineInfoElement
   Inherits XmlElement

   Friend Sub New(prefix As String, localname As String, nsURI As String, doc As XmlDocument)
      MyBase.New(prefix, localname, nsURI, doc)
      CType(doc, LineInfoDocument).IncrementElementCount()
   End Sub 'New
End Class 'LineInfoElement
 _ 'End LineInfoElement class.

Public Class Test

   Private filename As [String] = "book.xml"

   Public Shared Sub Main()

      Dim doc As New LineInfoDocument()
      doc.Load(filename)
      Console.WriteLine("Number of elements in {0}: {1}", filename, doc.GetCount())
   End Sub
End Class
```

```csharp
using System;
using System.Xml;
using System.IO;

class LineInfoDocument : XmlDocument {

  int elementCount;
  internal LineInfoDocument():base() {
    elementCount = 0;
  }

  public override XmlElement CreateElement( string prefix, string localname, string nsURI) {
    LineInfoElement elem = new LineInfoElement(prefix, localname, nsURI, this );
    return elem;
  }

  public void IncrementElementCount() {
     elementCount++;
  }

  public int GetCount() {
     return elementCount;
  }
} // End LineInfoDocument class.

class LineInfoElement:XmlElement {

    internal LineInfoElement( string prefix, string localname, string nsURI, XmlDocument doc ):base( prefix,localname,nsURI, doc ){
      ((LineInfoDocument)doc).IncrementElementCount();
    }
} // End LineInfoElement class.

public class Test {

  const String filename = "book.xml";
  public static void Main() {

     LineInfoDocument doc =new LineInfoDocument();
     doc.Load(filename);
     Console.WriteLine("Number of elements in {0}: {1}", filename, doc.GetCount());

  }
}
```

#### <a name="input"></a>输入

book.xml

```xml
<!--sample XML fragment-->
<book genre='novel' ISBN='1-861001-57-5' misc='sale-item'>
  <title>The Handmaid's Tale</title>
  <price>14.95</price>
</book>
```

#### <a name="output"></a>Output

```console
Number of elements in book.xml: 3
```

## <a name="node-event-handler"></a>节点事件处理程序

DOM 的 .NET Framework 实现还包括一个事件系统，使您能够在 XML 文档中的节点更改时接收并处理事件。 使用 <xref:System.Xml.XmlNodeChangedEventHandler> 和 <xref:System.Xml.XmlNodeChangedEventArgs> 类可以捕获 `NodeChanged`、`NodeChanging`、`NodeInserted`、`NodeInserting`、`NodeRemoved` 和 `NodeRemoving` 事件。

该事件处理过程在派生类中的工作方式同它在原始 DOM 类中的工作方式完全相同。

若要详细了解节点事件处理，请参阅[事件](../../events/index.md)和 <xref:System.Xml.XmlNodeChangedEventHandler>。

## <a name="default-attributes-and-the-createelement-method"></a>默认属性和 CreateElement 方法

如果要重写派生类中的 <xref:System.Xml.XmlDocument.CreateElement%2A> 方法，则在编辑文档期间创建新元素时不添加默认属性。 这只有在编辑时才是问题。 由于 <xref:System.Xml.XmlDocument.CreateElement%2A> 方法负责向 <xref:System.Xml.XmlDocument> 添加默认属性，因此必须在 <xref:System.Xml.XmlDocument.CreateElement%2A> 方法中编写此功能的代码。 如果要加载包含默认属性的 <xref:System.Xml.XmlDocument>，则这些属性将被正确处理。 若要详细了解默认属性，请参阅[新建 DOM 中元素的属性](creating-new-attributes-for-elements-in-the-dom.md)。

## <a name="see-also"></a>请参阅

- [XML 文档对象模型 (DOM)](xml-document-object-model-dom.md)
