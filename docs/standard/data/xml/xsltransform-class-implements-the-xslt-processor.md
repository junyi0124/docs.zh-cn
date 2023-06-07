---
title: XslTransform 类实现 XSLT 处理器
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 88373fe2-4a6b-44f9-8a62-8a3e348e3a46
ms.openlocfilehash: 57632321edf086b644da7ea4fca893edb589834f
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94818172"
---
# <a name="xsltransform-class-implements-the-xslt-processor"></a>XslTransform 类实现 XSLT 处理器

> [!NOTE]
> <xref:System.Xml.Xsl.XslTransform> 类在 .NET Framework 2.0 中已过时。 可以使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类执行可扩展样式表语言转换 (XSLT) 转换。 请参阅[使用 XslCompiledTransform 类](using-the-xslcompiledtransform-class.md)和[从 XslTransform 类迁移](migrating-from-the-xsltransform-class.md)，以获取详细信息。

<xref:System.Xml.Xsl.XslTransform> 类是实现 XSL 转换 (XSLT) 1.0 版建议的 XSLT 处理器。 <xref:System.Xml.Xsl.XslTransform.Load%2A> 方法定位并读取样式表，<xref:System.Xml.Xsl.XslTransform.Transform%2A> 方法转换给定的源文档。 任何实现了 <xref:System.Xml.XPath.IXPathNavigable> 接口的存储区都可以用作 <xref:System.Xml.Xsl.XslTransform> 的源文档。 .NET Framework 当前在 <xref:System.Xml.XmlDocument>、<xref:System.Xml.XmlDataDocument> 和 <xref:System.Xml.XPath.XPathDocument> 上实现了 <xref:System.Xml.XPath.IXPathNavigable> 接口，所以它们都可以用作转换的输入源文档。

.NET Framework 中的 <xref:System.Xml.Xsl.XslTransform> 对象仅支持用以下命名空间定义的 XSLT 1.0 规范：

```xml
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" version="1.0">
```

样式表可以用 <xref:System.Xml.Xsl.XslTransform.Load%2A> 方法从下列类之一中加载：

- XPathNavigator

- XmlReader

- 表示 URI 的字符串

上面的每个输入类分别有不同的 <xref:System.Xml.Xsl.XslTransform.Load%2A> 方法。 有些方法接受其中一个类与 <xref:System.Xml.XmlResolver> 类的组合作为参数。 <xref:System.Xml.XmlResolver> 定位由样式表中的 `<xsl:import>` 或 `<xsl:include>` 引用的资源。 下列方法以字符串、<xref:System.Xml.XmlReader> 或 <xref:System.Xml.XPath.XPathNavigator> 作为输入。

```vb
Overloads Public Sub Load(String)
```

```csharp
public void Load(string);
```

```vb
Overloads Public Sub Load(String, XmlResolver)
```

```csharp
public void Load(string, XmlResolver);
```

```vb
Overloads Public Sub Load(XmlReader, XmlResolver, Evidence)
```

```csharp
public void Load(XmlReader, XmlResolver, Evidence);
```

```vb
Overloads Public Sub Load(XPathNavigator, XmlResolver, Evidence)
```

```csharp
public void Load(XPathNavigator, XmlResolver, Evidence);
```

上面所示的多数 <xref:System.Xml.Xsl.XslTransform.Load%2A> 方法都接受 <xref:System.Xml.XmlResolver> 作为参数。 <xref:System.Xml.XmlResolver> 用于加载该样式表以及 xsl:import 和 xsl:include 元素中引用的任何样式表。

多数 <xref:System.Xml.Xsl.XslTransform.Load%2A> 方法还接受 evidence 作为参数。 evidence 参数是与样式表关联的 <xref:System.Security.Policy.Evidence>。 样式表的安全级别影响它引用的所有资源的安全级别，例如包含的脚本、使用的任何 `document()` 函数以及 <xref:System.Xml.Xsl.XsltArgumentList> 使用的任何扩展对象。

如果样式表是使用一个包含 URL 参数但没有提供证据的 <xref:System.Xml.Xsl.XslTransform.Load%2A> 方法加载的，该样式表的证据将通过结合给定的 URL 与其站点和区域计算得出。

如果未提供 URI 或证据，那么样式表的 证据集就完全受信任。 不要从不受信任的源加载样式表或将不受信任的扩展对象添加到 <xref:System.Xml.Xsl.XsltArgumentList>。

若要详细了解安全级别和证据及其对脚本的影响，请参阅[使用 \<msxsl:script> 编写 XSLT 样式表脚本](xslt-stylesheet-scripting-using-msxsl-script.md)。 若要了解安全级别和证据及其对扩展对象的影响，请参阅[样式表参数和扩展对象的 XsltArgumentList](xsltargumentlist-for-style-sheet-parameters-and-extension-objects.md)。

若要了解安全级别和证据及其对 `document()` 函数的影响，请参阅[解析外部 XSLT 样式表和文档](resolving-external-xslt-style-sheets-and-documents.md)。

可以给样式表提供许多输入参数。 样式表也可以调用扩展对象上的函数。 参数和扩展对象都是使用 <xref:System.Xml.Xsl.XsltArgumentList> 类提供给样式表的。 有关 <xref:System.Xml.Xsl.XsltArgumentList> 的详细信息，请参阅<xref:System.Xml.Xsl.XsltArgumentList>。

## <a name="recommended-secure-use-of-xsltransform-class"></a>XslTransform 类建议的安全用法

样式表的安全特权取决于提供的证据。 下表概括了样式表的位置并说明了应提供的证据类型。

- XSLT 样式表没有外部引用，或者样式表来自你信任的代码库。

  - 提供源自您的程序集的证据：

    ```vb
    Dim xslt = New XslTransform() xslt.Load(stylesheet, resolver, Me.GetType().Assembly.Evidence)

    XsltTransform xslt = new XslTransform();  xslt.Load(stylesheet, resolver, this.GetType().Assembly.Evidence);
    ```

- XSLT 样式表来自外部源。 源的来源已知而且有一个可验证的 URI。

  - 用 URI 创建证据。

    ```vb
    Dim xslt As New XslTransform() Dim ev As Evidence = XmlSecureResolver.CreateEvidenceForUrl(stylesheetUri) xslt.Load(stylesheet, resolver, evidence)

    XslTransform xslt = new XslTransform(); Evidence ev = XmlSecureResolver.CreateEvidenceForUrl(stylesheetUri); xslt.Load(stylesheet, resolver, evidence);
    ```

- XSLT 样式表来自外部源。 源的来源未知。

  - 将 evidence 设置为 `null`。 脚本块将不进行处理，不支持 XSLT `document()` 函数，而且不允许特权扩展对象。

    此外，您还可以将 `resolver` 参数设置为 `null`。这可确保不处理 `xsl:import` 和 `xsl:include` 元素。

- XSLT 样式表来自外部源。 源的来源未知，但您需要脚本支持。

  - 从调用方请求证据。

## <a name="transformation-of-xml-data"></a>XML 数据的转换

加载样式表后，转换过程将调用一个 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 方法并提供输入源文档，开始进行转换。 重载 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 方法以提供不同的转换输出。 转换可产生下列输出格式：

- <xref:System.Xml.XmlReader>

- <xref:System.Xml.XmlWriter>

- <xref:System.IO.TextWriter>

- <xref:System.IO.Stream>

- 文件的字符串 URL

最后一种格式是字符串 URL，为转换位于 URL 中的输入文档并将文档写入输出 URL 提供了一种常用方案。 此 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 方法是从文件加载 XML 文档、执行 XSLT 转换并将输出写入文件的便捷方法。 这样，您就不必创建并加载输入源文档，然后写入文件流。 下面的代码示例显示了 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 方法的用法，使用字符串 URL 用作输入和输出：

```vb
Dim xsltransform As XslTransform = New XslTransform()
xsltransform.Load("favorite.xsl")
xsltransform.Transform("MyDocument.Xml", "TransformResult.xml", Nothing)
```

```csharp
XslTransform xsltransform = new XslTransform();
xsltransform.Load("favorite.xsl");
xsltransform.Transform("MyDocument.xml", "TransformResult.xml", null);
```

## <a name="transforming-a-section-of-an-xml-document"></a>转换 XML 文档的节

转换将应用于整个文档。 换句话说，如果你传入文档根节点以外的一个节点，并不能防止转换进程访问已加载文档的所有节点。 若要转换结果树片段，必须创建一个仅包含结果树片段的 <xref:System.Xml.XmlDocument>，并将该 <xref:System.Xml.XmlDocument> 传递给 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 方法。 下面的示例对一个结果树片段执行转换。

```vb
Dim xslt As New XslTransform()
xslt.Load("print_root.xsl")
Dim doc As New XmlDocument()
doc.Load("library.xml")
' Create a new document containing just the result tree fragment.
Dim testNode As XmlNode = doc.DocumentElement.FirstChild
Dim tmpDoc As New XmlDocument()
tmpDoc.LoadXml(testNode.OuterXml)
' Pass the document containing the result tree fragment
' to the Transform method.
Console.WriteLine(("Passing " + tmpDoc.OuterXml + " to print_root.xsl"))
xslt.Transform(tmpDoc, Nothing, Console.Out, Nothing)
```

```csharp
XslTransform xslt = new XslTransform();
xslt.Load("print_root.xsl");
XmlDocument doc = new XmlDocument();
doc.Load("library.xml");
// Create a new document containing just the result tree fragment.
XmlNode testNode = doc.DocumentElement.FirstChild;
XmlDocument tmpDoc = new XmlDocument();
tmpDoc.LoadXml(testNode.OuterXml);
// Pass the document containing the result tree fragment
// to the Transform method.
Console.WriteLine("Passing " + tmpDoc.OuterXml + " to print_root.xsl");
xslt.Transform(tmpDoc, null, Console.Out, null);
```

该示例使用 library.xml 和 print_root.xsl 文件作为输入并将以下内容输出到控制台：

```console
Passing <book genre="novel" ISBN="1-861001-57-5"><title>Pride And Prejudice</title></book> to print_root.xsl
Root node is book.
```

library.xml

```xml
<library>
  <book genre='novel' ISBN='1-861001-57-5'>
     <title>Pride And Prejudice</title>
  </book>
  <book genre='novel' ISBN='1-81920-21-2'>
     <title>Hook</title>
  </book>
</library>
```

print_root.xsl

```xml
<stylesheet version="1.0" xmlns="http://www.w3.org/1999/XSL/Transform" >
  <output method="text" />
  <template match="/">
     Root node is  <value-of select="local-name(//*[position() = 1])" />
  </template>
</stylesheet>
```

## <a name="migration-of-xslt-from-net-framework-version-10-to-net-framework-version-11"></a>XSLT 从 .NET Framework 1.0 版到 .NET Framework 1.1 版的迁移

下表显示 <xref:System.Xml.Xsl.XslTransform.Load%2A> 方法的已过时的 .NET Framework 1.0 版方法以及新的 .NET Framework 1.1 方法。 新方法允许您通过指定证据来限制样式表的权限。

|.NET Framework 版本 1.0 中已过时的 Load 方法|.NET Framework 版本 1.1 中替换的新 Load 方法|
|------------------------------------------------------|---------------------------------------------------------|
|Load(XPathNavigator input);<br /><br /> Load(XPathNavigator input, XmlResolver resolver);|Load(XPathNavigator stylesheet, XmlResolver resolver, Evidence evidence);|
|Load(IXPathNavigable stylesheet);<br /><br /> Load(IXPathNavigable stylesheet, XmlResolver resolver);|Load(IXPathNavigable stylesheet, XmlResolver resolver, Evidence evidence);|
|Load(XmlReader stylesheet);<br /><br /> Load(XmlReader stylesheet, XmlResolver resolver);|Load(XmlReader stylesheet, XmlResolver resolver, Evidence evidence);|

下表显示 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 方法的已过时方法和新方法。 新方法接受 <xref:System.Xml.XmlResolver> 对象。

|.NET Framework 版本 1.0 中已过时的 Transform 方法|.NET Framework 版本 1.1 中替换的新 Transform 方法|
|-----------------------------------------------------------|--------------------------------------------------------------|
|XmlReader Transform(XPathNavigator input, XsltArgumentList args)|XmlReader Transform(XPathNavigator input, XsltArgumentList args, XmlResolver resolver)|
|XmlReader Transform(IXPathNavigable input, XsltArgumentList args)|XmlReader Transform(IXPathNavigable input, XsltArgumentList args, XmlResolver resolver)|
|Void Transform(XPathNavigator input, XsltArgumentList args, XmlWriter output)|Void Transform(XPathNavigator input, XsltArgumentList args, XmlWriter output, XmlResolver resolver)|
|Void Transform(IXPathNavigable input, XsltArgumentList args, XmlWriter output)|Void Transform(IXpathNavigable input, XsltArgumentList args, XmlWriter output, XmlResolver resolver)|
|Void Transform(XPathNavigator input, XsltArgumentList args, TextWriter output)|Void Transform(XPathNavigator input, XsltArgumentList args, TextWriter output, XmlResolver resolver)|
|Void Transform(IXPathNavigable input, XsltArgumentList args, TextWriter output)|Void Transform(IXPathNavigable input, XsltArgumentList args, TextWriter output, XmlResolver resolver)|
|Void Transform(XPathNavigator input, XsltArgumentList args, Stream output)|Void Transform(XPathNavigator input, XsltArgumentList args, Stream output, XmlResolver resolver)|
|Void Transform(IXPathNavigable input, XsltArgumentList args, Stream output)|Void Transform(IXPathNavigable input, XsltArgumentList args, Stream output, XmlResolver resolver)|
|Void Transform(String input, String output);|Void Transform(String input, String output, XmlResolver resolver);|

<xref:System.Xml.Xsl.XslTransform.XmlResolver%2A?displayProperty=nameWithType> 属性在 .NET Framework 1.1 版中已过时。 应改用接受 <xref:System.Xml.Xsl.XslTransform.Transform%2A> 对象的新 <xref:System.Xml.XmlResolver> 重载。

## <a name="see-also"></a>请参阅

- <xref:System.Xml.Xsl.XslTransform>
- [XslTransform 类的 XSLT 转换](xslt-transformations-with-the-xsltransform-class.md)
- [转换中的 XPathNavigator](xpathnavigator-in-transformations.md)
- [转换中的 XPathNodeIterator](xpathnodeiterator-in-transformations.md)
- [XslTransform 的 XPathDocument 输入](xpathdocument-input-to-xsltransform.md)
- [XslTransform 的 XmlDataDocument 输入](xmldatadocument-input-to-xsltransform.md)
- [XslTransform 的 XmlDocument 输入](xmldocument-input-to-xsltransform.md)
