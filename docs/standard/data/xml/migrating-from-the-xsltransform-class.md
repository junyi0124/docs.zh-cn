---
title: 从 XslTransform 类迁移
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 9404d758-679f-4ffb-995d-3d07d817659e
ms.openlocfilehash: b441e23b13983a0fdb54b7785e249a04bf1407c8
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830202"
---
# <a name="migrating-from-the-xsltransform-class"></a>从 XslTransform 类迁移

Visual Studio 2005 版本重新设计了 XSLT 体系结构。 <xref:System.Xml.Xsl.XslTransform> 类已由 <xref:System.Xml.Xsl.XslCompiledTransform> 类取代。

以下各节说明 <xref:System.Xml.Xsl.XslCompiledTransform> 和 <xref:System.Xml.Xsl.XslTransform> 类之间的一些主要区别。

## <a name="performance"></a>性能

<xref:System.Xml.Xsl.XslCompiledTransform> 类在性能方面进行了许多改进。 新的 XSLT 处理器将 XSLT 样式表编译为公共中间格式，与公共语言运行库 (CLR) 对其他编程语言的操作类似。 样式表编译后，可以缓存并重复使用。

<xref:System.Xml.Xsl.XslCompiledTransform> 类还进行了其他优化，使其比 <xref:System.Xml.Xsl.XslTransform> 类快得多。

> [!NOTE]
> 尽管 <xref:System.Xml.Xsl.XslCompiledTransform> 类的总体性能优于 <xref:System.Xml.Xsl.XslTransform> 类，但在首次对转换调用时，<xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> 类的 <xref:System.Xml.Xsl.XslCompiledTransform> 方法可能比 <xref:System.Xml.Xsl.XslTransform.Load%2A> 类的 <xref:System.Xml.Xsl.XslTransform> 方法慢。 这是因为必须先编译 XSLT 文件，才能加载该文件。 有关详细信息，请参阅以下博客文章：[XslCompiledTransform Slower than XslTransform?](/archive/blogs/antosha/xslcompiledtransform-slower-than-xsltransform)（XslCompiledTransform 比 XslTransform 慢？）

## <a name="security"></a>安全性

默认情况下，<xref:System.Xml.Xsl.XslCompiledTransform> 类禁用对 XSLT `document()` 函数和嵌入式脚本的支持。 通过创建启用这些功能的 <xref:System.Xml.Xsl.XsltSettings> 对象并将其传递给 <xref:System.Xml.Xsl.XslCompiledTransform.Load%2A> 方法，可以启用这些功能。 下面的示例演示如何启用脚本并执行 XSLT 转换。

[!code-csharp[XML_Migration#16](../../../../samples/snippets/csharp/VS_Snippets_Data/XML_Migration/CS/migration.cs#16)]
[!code-vb[XML_Migration#16](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XML_Migration/VB/migration.vb#16)]

有关详细信息，请参阅 [XSLT 安全注意事项](xslt-security-considerations.md)。

## <a name="new-features"></a>新增功能

### <a name="temporary-files"></a>临时文件

在 XSLT 处理过程中有时会生成临时文件。 如果样式表包含脚本块或是在将调试设置设为 true 时进行编译的，则可能会在 %TEMP% 文件夹中创建临时文件。 可能会存在由于计时问题而不删除某些临时文件的情况。 例如，如果文件正在由当前的 AppDomain 或调试器使用，则 <xref:System.CodeDom.Compiler.TempFileCollection> 对象的终结器将无法移除这些文件。

<xref:System.Xml.Xsl.XslCompiledTransform.TemporaryFiles%2A> 属性可用于进行附加清理，以确保从客户端中移除所有临时文件。

### <a name="support-for-the-xsloutput-element-and-xmlwriter"></a>对 xsl:output 元素和 XmlWriter 的支持

在将转换输出发送到 <xref:System.Xml.Xsl.XslTransform> 对象时，`xsl:output` 类忽略 <xref:System.Xml.XmlWriter> 设置。 <xref:System.Xml.Xsl.XslCompiledTransform> 类具有一个 <xref:System.Xml.Xsl.XslCompiledTransform.OutputSettings%2A> 属性，它可返回一个 <xref:System.Xml.XmlWriterSettings> 对象，该对象包含从样式表的 `xsl:output` 元素派生的输出信息。 <xref:System.Xml.XmlWriterSettings> 对象用于创建具有正确设置的 <xref:System.Xml.XmlWriter> 对象，可将该对象传递给 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法。 下面的 C# 代码阐释这一行为：

```csharp
// Create the XslTransform object and load the style sheet.
XslCompiledTransform xslt = new XslCompiledTransform();
xslt.Load(stylesheet);

// Load the file to transform.
XPathDocument doc = new XPathDocument(filename);

// Create the writer.
XmlWriter writer = XmlWriter.Create(Console.Out, xslt.OutputSettings);

// Transform the file and send the output to the console.
xslt.Transform(doc, writer);
writer.Close();
```

### <a name="debug-option"></a>调试选项

<xref:System.Xml.Xsl.XslCompiledTransform> 类可生成调试信息，让用户能够使用 Microsoft Visual Studio 调试器调试样式表。 有关更多信息，请参见<xref:System.Xml.Xsl.XslCompiledTransform.%23ctor%28System.Boolean%29>。

## <a name="behavioral-differences"></a>行为差异

### <a name="transforming-to-an-xmlreader"></a>转换为 XmlReader

<xref:System.Xml.Xsl.XslTransform> 类具有多个作为 <xref:System.Xml.XmlReader> 对象返回转换结果的 Transform 重载。 这些重载可用于将转换结果加载到内存表示形式（如 <xref:System.Xml.XmlDocument> 或 <xref:System.Xml.XPath.XPathDocument>），而不会增加对生成的 XML 树进行序列化和反序列化所造成的开销。 下面的 C# 代码演示如何将转换结果加载到 <xref:System.Xml.XmlDocument> 对象。

```csharp
// Load the style sheet
XslTransform xslt = new XslTransform();
xslt.Load("MyStylesheet.xsl");

// Transform input document to XmlDocument for additional processing
XmlDocument doc = new XmlDocument();
doc.Load(xslt.Transform(input, (XsltArgumentList)null));
```

<xref:System.Xml.Xsl.XslCompiledTransform> 类不支持转换为 <xref:System.Xml.XmlReader> 对象。 不过，通过使用 <xref:System.Xml.XPath.XPathNavigator.CreateNavigator%2A> 方法直接从 <xref:System.Xml.XmlWriter> 中加载生成的 XML 树，你可以执行类似操作。 下面的 C# 代码演示如何使用 <xref:System.Xml.Xsl.XslCompiledTransform> 完成相同的任务。

```csharp
// Transform input document to XmlDocument for additional processing
XmlDocument doc = new XmlDocument();
using (XmlWriter writer = doc.CreateNavigator().AppendChild()) {
    xslt.Transform(input, (XsltArgumentList)null, writer);
}
```

### <a name="discretionary-behavior"></a>任意行为

W3C XSL 转换 (XSLT) 1.0 版建议中涉及到实现提供者可以在哪些方面确定如何处理某种情况。 这些方面被认为是任意行为。 在有些方面，<xref:System.Xml.Xsl.XslCompiledTransform> 行为不同于 <xref:System.Xml.Xsl.XslTransform> 类。 有关详细信息，请参阅[可恢复的 XSLT 错误](recoverable-xslt-errors.md)。

### <a name="extension-objects-and-script-functions"></a>扩展对象和脚本函数

<xref:System.Xml.Xsl.XslCompiledTransform> 介绍对使用脚本函数的两个新限制：

- 从 XPath 表达式中只能调用公共方法。

- 重载之间可以基于自变量数量进行区分。 如果多个重载具有相同数量的参数，则将引发异常。

在 <xref:System.Xml.Xsl.XslCompiledTransform> 中，脚本函数的绑定（方法名称查找）发生在编译时，当使用 <xref:System.Xml.Xsl.XslCompiledTransform> 加载由 XslTranform 使用的样式表时，这些样式表可能会引发异常。

<xref:System.Xml.Xsl.XslCompiledTransform> 支持在 `msxsl:using` 元素中具有 `msxsl:assembly` 和 `msxsl:script` 子元素。 `msxsl:using` 和 `msxsl:assembly` 元素用于声明其他命名空间和程序集，以便在脚本块中使用。 有关详细信息，请参阅[使用 msxsl:script 的脚本块](script-blocks-using-msxsl-script.md)。

<xref:System.Xml.Xsl.XslCompiledTransform> 禁止扩展对象具有相同数量的自变量的多个重载。

### <a name="msxml-functions"></a>MSXML 函数

已向 <xref:System.Xml.Xsl.XslCompiledTransform> 类添加对其他 MSXML 函数的支持。 下面的列表说明新增或改进功能：

- msxsl:node-set：<xref:System.Xml.Xsl.XslTransform> 要求 [node-set Function](/previous-versions/dotnet/netframework-4.0/ms256197(v=vs.100)) 函数的参数是结果树片段。 <xref:System.Xml.Xsl.XslCompiledTransform> 类没有此需求。

- msxsl:version:<xref:System.Xml.Xsl.XslCompiledTransform> 支持此函数。

- XPath 扩展函数：现在支持 [ms:string-compare Function](/previous-versions/dotnet/netframework-4.0/ms256114(v=vs.100))、[ms:utc Function](/previous-versions/dotnet/netframework-4.0/ms256474(v=vs.100))、[ms:namespace-uri Function](/previous-versions/dotnet/netframework-4.0/ms256231(v=vs.100))、[ms:local-name Function](/previous-versions/dotnet/netframework-4.0/ms256055(v=vs.100))、[ms:number Function](/previous-versions/dotnet/netframework-4.0/ms256155(v=vs.100))、[ms:format-date Function](/previous-versions/dotnet/netframework-4.0/ms256099(v=vs.100)) 和 [ms:format-time Function](/previous-versions/dotnet/netframework-4.0/ms256467(v=vs.100)) 函数。

- 与架构相关的 XPath 扩展函数：<xref:System.Xml.Xsl.XslCompiledTransform> 不再支持这些函数。 但是，它们可以作为扩展函数实现。

## <a name="see-also"></a>请参阅

- [XSLT 转换](xslt-transformations.md)
- [使用 XslCompiledTransform 类](using-the-xslcompiledtransform-class.md)
