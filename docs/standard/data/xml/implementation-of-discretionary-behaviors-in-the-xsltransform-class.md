---
title: XslTransform 类中任意行为的实现
ms.date: 03/30/2017
ms.assetid: d2758ea1-03f6-47bd-88d2-0fb7ccdb2fab
ms.openlocfilehash: 7617970ead6c2d75b7ca9749578ba66efec311bc
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829409"
---
# <a name="implementation-of-discretionary-behaviors-in-the-xsltransform-class"></a>XslTransform 类中任意行为的实现

> [!NOTE]
> <xref:System.Xml.Xsl.XslTransform> 类在 .NET Framework 2.0 中已过时。 可以使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类执行可扩展样式表语言转换 (XSLT) 转换。 请参阅[使用 XslCompiledTransform 类](using-the-xslcompiledtransform-class.md)和[从 XslTransform 类迁移](migrating-from-the-xsltransform-class.md)，以获取详细信息。

下面将介绍在[万维网联合会 (W3C) XSL 转换 (XSLT) 1.0 版建议](https://www.w3.org/TR/1999/REC-xslt-19991116)中列出的一些任意行为，在这些行为中，实现提供者选择几个可能的选项中的一个作为处理某种情况的方法。 例如，在第 7.3 节“Creating Processing Instructions”中，W3C 建议指出，如果实例化 `xsl:processing-instruction` 的内容会创建文本节点以外的节点，则会发生错误。 对于某些问题，W3C 说明了在处理器决定从错误中恢复时应做的决策。 对于 7.3 节中给出的问题，W3C 指出，实现可以通过忽略节点及其内容来从此错误中恢复。

因此，对 W3C 允许的每一种任意行为，下表列出了为 <xref:System.Xml.Xsl.XslTransform> 类的 .NET Framework 实现而实现的任意行为，并说明了此问题在 W3C XSLT 1.0 建议中的哪一节讨论。

|问题|行为|节|
|-------------|--------------|-------------|
|文本节点同时与 `xsl:strip-space` 和 `xsl:preserve-space` 匹配。|恢复|3.4|
|源节点与多个模板规则匹配。|恢复|5.5|
|某个命名空间统一资源标识符 (URI) 声明为多个命名空间 URI 的别名，所有这些 URI 都具有相同的导入优先级。|恢复|7.1.1|
|从属性值模板生成的 `xsl:attribute` 和 `xsl:element` 中的名称属性不是有效的限定名 (QName)。|引发异常|7.1.2 和 7.1.3|
|在一个元素节点已添加了子节点后向此元素添加属性。|恢复|7.1.3|
|将属性添加到元素节点以外的任何内容中。|恢复|7.1.3|
|`xsl:attribute` 元素的内容实例化不是文本节点。|恢复|7.1.3|
|两个属性集具有相同的导入优先级和扩展名称。 两个属性集具有相同的属性，并且没有其他属性集包含导入优先级更高的同名公共属性。|恢复|7.1.4|
|`xsl:processing-instruction` 名称属性不同时产生无冒号名称 (NCName) 和处理指令目标。|恢复|7.3|
|实例化 `xsl:processing-instruction` 的内容创建了文本节点以外的节点。|恢复|7.3|
|`xsl:processing-instruction` 内容的实例化结果中包含字符串“`?>`”。|恢复|7.3|
|`xsl:comment` 内容的实例化结果中包含字符串“--”，或以“-”结尾。|恢复|7.4|
|`xsl:comment` 内容的实例化结果创建了文本节点以外的节点。|恢复|7.4|
|变量绑定元素内部的模板返回属性节点或命名空间节点。|恢复|11.2|
|从传入文档函数的 URI 中检索资源时出错。|引发异常|12.1|
|文档函数中的 URI 引用包含片断标识符，在处理段落标识符时出错。|引发异常|12.1|
|`cdata-section-elements` 中有多个未命名为 `xls:output` 的同名属性，且这些属性具有相同的导入优先级。|恢复|16|
|处理器不支持 `encoding` 元素的 `xsl:output` 属性中给出的字符编码值。|恢复|16.1|
|`disable-output-escaping` 用于一个文本节点，而该文本节点用于在结果树中创建文本节点以外的内容。|`disable-output-escaping` 属性被忽略|16.4|
|如果结果树片段包含启用了输出转义的文本节点，则将该结果树片断转换为数字或字符串。|忽略|16.4|
|对不能以 XSLT 处理器用于输出的编码表示的字符禁用输出转义。|忽略|16.4|
|向元素添加子级或添加属性后，向元素添加命名空间节点。|恢复|勘误表 e25|
|`xsl:number` 为 NaN、无限大或小于 0.5。|恢复|勘误表 e24|
|文档函数的第二个参数 node-set 为空，且 URI 引用是相对的。|恢复|勘误表 e14|

可在 W3C [XSL 转换 (XSLT) 1.0 版规范勘误表](https://www.w3.org/1999/11/REC-xslt-19991116-errata/)中找到此勘误表的节。

## <a name="custom-defined-implementation-behaviors"></a>自定义实现行为

<xref:System.Xml.Xsl.XslTransform> 类实现具有独特的行为。 本节讨论提供程序特定的 `xsl:sort` 实现和 <xref:System.Xml.Xsl.XslTransform> 类支持的可选功能。

## <a name="xslsort"></a>xsl:sort

使用转换进行排序时，W3C XSLT 1.0 建议提供几点说明。 它们是：

- 两个 XSLT 处理器可能是一致的处理器，但仍可能以不同的方式排序。

- 并非所有 XSLT 处理器都支持相同的语言。

- 对于语言，不同处理器的排序方式可能因特定的语言而异，而不是按照 `xsl:sort.` 指定的方式。

下表展示了 .NET Framework 用 <xref:System.Xml.Xsl.XslTransform> 实现的转换中为每种数据类型实现的排序行为：

|数据类型|排序行为|
|---------------|----------------------|
|Text|数据通过公共语言运行库 (CLR) String.Compare 方法和区域设置进行排序。 当数据类型为“text”时，<xref:System.Xml.Xsl.XslTransform> 类的排序行为与 CLR 的字符串比较行为相同。|
|数字|数值被视为 XML 路径语言 (XPath) 数字，并根据 W3C [XML 路径语言 (XPath) 1.0 版建议的 3.5 节](https://www.w3.org/TR/1999/REC-xpath-19991116/#numbers) 列出的详细信息进行排序。|

## <a name="optional-features-supported"></a>支持的可选功能

下表显示 XSLT 处理器实现的可选功能，这些功能在 <xref:System.Xml.Xsl.XslTransform> 类中实现。

|功能|引用位置|说明|
|-------------|------------------------|-----------|
|`disable-output-escaping` 和 `<xsl:text...>` 标记上的 `<xsl:value-of...>` 属性。|W3C XSLT 1.0 建议，<br /><br /> 16.4 节|当在 `disable-output-escaping`、`xsl:text` 或 `xsl:value-of` 元素中使用 `xsl:comment` 或 `xsl:processing-instruction` 元素时，忽略 `xsl:attribute` 属性。<br /><br /> 不支持包含文本的结果树片段和已转义的文本输出。<br /><br /> disable-output-escaping 属性在转换到 <xref:System.Xml.XmlReader> 或 <xref:System.Xml.XmlWriter> 对象时被忽略。|

## <a name="see-also"></a>请参阅

- <xref:System.Xml.Xsl.XslTransform>
- [XslTransform 类实现 XSLT 处理器](xsltransform-class-implements-the-xslt-processor.md)
- [XslTransform 类的 XSLT 转换](xslt-transformations-with-the-xsltransform-class.md)
- [转换中的 XPathNavigator](xpathnavigator-in-transformations.md)
- [转换中的 XPathNodeIterator](xpathnodeiterator-in-transformations.md)
- [XslTransform 的 XPathDocument 输入](xpathdocument-input-to-xsltransform.md)
- [XslTransform 的 XmlDataDocument 输入](xmldatadocument-input-to-xsltransform.md)
- [XslTransform 的 XmlDocument 输入](xmldocument-input-to-xsltransform.md)
