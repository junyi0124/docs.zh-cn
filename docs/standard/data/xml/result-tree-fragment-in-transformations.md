---
title: 转换中的结果树片断
ms.date: 03/30/2017
ms.assetid: df363480-ba02-4233-9ddf-8434e421c4f1
ms.openlocfilehash: e5ca285c0b5d6c32d15647508c611a47ed97ee03
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94823558"
---
# <a name="result-tree-fragment-in-transformations"></a>转换中的结果树片断

> [!NOTE]
> <xref:System.Xml.Xsl.XslTransform> 类在 .NET Framework 2.0 中已过时。 可以使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类执行可扩展样式表语言转换 (XSLT) 转换。 请参阅[使用 XslCompiledTransform 类](using-the-xslcompiledtransform-class.md)和[从 XslTransform 类迁移](migrating-from-the-xsltransform-class.md)，以获取详细信息。

 结果树片段只是一个特殊类型的节点集。 您可以对它们执行可对节点集执行的任何函数。 你也可以用 `node-set()` 函数将结果树片段转换成一个节点集，然后就可以在任何可使用节点集的位置使用了。

 结果树片段是通过在样式表中以一种特定方式使用 `<xsl:variable>` 或 `<xsl:param>` 元素而创建的。 `variable` 和 `parameter` 元素的语法如下所示：

```xml
<xsl:param name=Qname select= XPath Expression >
    template body
</xsl:param>

<xsl:variable name=Qname select=XPath Expression >
    template body
</xsl:variable>
```

对于 `parameter` 元素，可以用几种方式给限定名 (`Qname`) 赋值。 您可以通过如下方式为该参数分配默认值：从 `select` 属性中的 XML 路径语言 (XPath) 表达式返回内容，或者将模板正文的内容分配给该参数。

对于 `variable` 元素，赋值方法也有多种。 您可以通过从 `select` 属性中的 XPath 表达式返回内容或通过将模板正文的内容分配给该参数来进行赋值。

对于 `parameter` 和 `variable` 这两个元素，如果通过 XPath 表达式赋值，则将返回四种基本 XPath 类型之一：布尔值、字符串、数字或节点集。 如果是通过非空模板正文赋值，则会返回非 XPath 数据类型，而且将是一个结果树片段。

只有当变量绑定到结果树片段而非这四种基本 XPath 数据类型之一时，XPath 查询才会返回这四种 XPath 对象类型以外的类型。 结果树片段及其行为在[万维网联合会 (W3C) 规范](https://www.w3.org/TR/xslt-10/)[第 11.1 节“Result Tree Fragments”](https://www.w3.org/TR/xslt-10/#section-Result-Tree-Fragments)至[第 11.6 节“Passing Parameters to Templates”](https://www.w3.org/TR/xslt-10/#section-Passing-Parameters-to-Templates)中讲述。 另外，[第 1 节“Introduction”](https://www.w3.org/TR/xslt-10/#section-Introduction)讲述了模板如何包含 XSLT 命名空间中返回或创建结果树片段的元素。

从概念上说，结果树片段的行为就像一个只有单个根节点的节点集。 不过，返回的其余节点都是子节点。 若要以编程方式查看子节点，请使用 `<xsl:copy-of>` 元素将结果树片段复制到结果树中。 在执行 copy-of 时，所有子节点也都按顺序复制到结果树中。 在使用 `copy` 或 `copy-of` 之前，结果树片段不是结果树或转换输出的一部分。

若要循环访问结果树片段的返回节点，请使用 <xref:System.Xml.XPath.XPathNavigator>。 下面的代码示例显示了如何通过调用具有参数 `fragment`（包含 XML）的函数在样式表内创建一个结果树片段。

```xml
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
                xmlns:msxsl="urn:schemas-microsoft-com:xslt"
                xmlns:user="http://www.adventure-works.com"
                version="1.0">
    <xsl:var name="fragment">
        <node1>
            <node2/>
        </node1>
    <xsl:var>

  <msxsl:script language="C#" implements-prefix="user">
    function NodeFragment(XPathNavigator nav)
    {
      if (nav.HasSelection == false)
        XPathNavigator.MoveToNext();
      return;
    }
  </msxsl:script>

    <xsl:template match="/">
            <xsl:value-of select="user:NodeFragment(msxml:node-set($fragment))"/>
    </xsl:template>
</xsl:stylesheet>
```

下面是另一个显示变量的示例，此变量的格式为 RTF（丰富文本格式），因而一种结果树片段类型，未转换为节点集， 而是传递给一个脚本函数，<xref:System.Xml.XPath.XPathNavigator> 用来在这些节点上浏览。

```xml
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
        xmlns:msxsl="urn:schemas-microsoft-com:xslt"
        xmlns:user="urn:books"
        exclude-result-prefixes="msxsl">

<xsl:output method="xml" indent="yes" omit-xml-declaration="yes"/>

<xsl:variable name="node-fragment">
    <book>Book1</book>
    <book>Book2</book>
    <book>Book3</book>
    <book>Book4</book>
</xsl:variable>

<msxsl:script implements-prefix="user" language="c#">

<![CDATA[
    string func(XPathNavigator nav)
    {
        bool b = nav.MoveToFirstChild();
        if (b)
            return nav.Value;
        else
            return "Does not exist";
    }

]]>

</msxsl:script>

<xsl:template match="/">
    <first_book>
        <xsl:value-of select="user:func($node-fragment)"/>
    </first_book>
</xsl:template>

</xsl:stylesheet>
```

用此样式表转换任何 XML 的结果显示在下面的输出中。

## <a name="output"></a>Output

```xml
<first_book xmlns:user="urn:books">Book1</first_book>
```

如前所述，`node-set` 函数可以将结果树片段转换成节点集。 生成的节点集总是包含单个节点并且是树的根节点。 如果将结果树片段转换成节点集，就可以在任何使用常规节点集的位置使用，如在 for-each 语句或在 `select` 属性的值中。 下面的代码行显示了被转换为节点集并作为节点集使用的片段：

`<xsl:for-each select="msxsl:node-set($node-fragment)">`

`<xsl:value-of select="user:func(msxsl:node-set($node-fragment))"/>`

当片段转换成节点集后，就再也不用 <xref:System.Xml.XPath.XPathNavigator> 进行浏览。 对于节点集，应改用 <xref:System.Xml.XPath.XPathNodeIterator>。

在下面的示例中，`$var` 是一个变量，是样式表中的一个节点树。 for-each 语句与 `node-set` 函数组合使用，允许用户将此树作为节点集循环访问。

```xml
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
                xmlns:msxsl="urn:schemas-microsoft-com:xslt"
                xmlns:user="http://www.adventure-works.com"
                version="1.0">
    <xsl:variable name="states">
        <node1>AL</node1>
        <node1>CA</node1>
        <node1>CO</node1>
        <node1>WA</node1>
    </xsl:variable>

    <xsl:template match="/">
            <xsl:for-each select="msxsl:node-set($states)"/>
    </xsl:template>
</xsl:stylesheet>
```

下面是另一个变量示例，该变量格式为 RTF，因而是节点树片段类型，在作为 XPathNodeIterator 传递给一个脚本函数前转换成了节点集。

```xml
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
        xmlns:msxsl="urn:schemas-microsoft-com:xslt"
        xmlns:user="urn:books"
        exclude-result-prefixes="msxsl">

<xsl:output method="xml" indent="yes" omit-xml-declaration="yes"/>

<xsl:variable name="node-fragment">
    <book>Book1</book>
    <book>Book2</book>
    <book>Book3</book>
    <book>Book4</book>
</xsl:variable>

<msxsl:script implements-prefix="user" language="c#">

<![CDATA[
    string func(XPathNodeIterator it)
    {
        it.MoveNext();
        return it.Current.Value;
        //it.Current returns XPathNavigator positioned on the current node
    }

]]>

</msxsl:script>
<xsl:template match="/">
    <books>
        <xsl:value-of select="user:func(msxsl:node-set($node-fragment))"/>
    </books>
</xsl:template>

</xsl:stylesheet>
```

用此样式表转换 XML 的结果如下所示：

```xml
<books xmlns:user="urn:books">Book1Book2Book3Book4</books>
```

## <a name="see-also"></a>请参阅

- <xref:System.Xml.XPath.XPathNodeIterator>
- [XslTransform 类的 XSLT 转换](xslt-transformations-with-the-xsltransform-class.md)
- [XslTransform 类实现 XSLT 处理器](xsltransform-class-implements-the-xslt-processor.md)
