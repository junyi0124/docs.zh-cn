---
title: 转换中的节点集
ms.date: 03/30/2017
ms.assetid: ad034f0e-ff8b-4a71-9a4c-528c754263c4
ms.openlocfilehash: 2ace103d9721b2adecb08a25ab89cf9b0f05bb93
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734109"
---
# <a name="node-sets-in-transformations"></a>转换中的节点集

节点集是从 XML 路径语言 (XPath) 表达式返回的四种基本数据类型之一。 节点集是按文档顺序创建的无重复节点的无序集合，可将其分配给样式表中的某个变量。  
  
> [!NOTE]
> <xref:System.Xml.Xsl.XslTransform> 类在 .NET Framework 2.0 中已过时。 可以使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类执行可扩展样式表语言转换 (XSLT) 转换。 请参阅[使用 XslCompiledTransform 类](using-the-xslcompiledtransform-class.md)和[从 XslTransform 类迁移](migrating-from-the-xsltransform-class.md)，以获取详细信息。  
  
 节点集是从 XPath 表达式返回的四种基本数据类型之一。 节点集是按文档顺序创建的无重复节点的无序集合，可将其分配给样式表中的某个变量。 此节点集是在转换中的 `select` 属性中使用的 XPath 表达式的结果，它与 XML 文档对象模型 (DOM) 中的节点集有相同的行为。 可以使用[使用 XPathNavigator 的节点集定位](node-set-navigation-using-xpathnavigator.md)中介绍的一组方法浏览节点集，与结果树片断不同，后者使用 <xref:System.Xml.XPath.XPathNodeIterator> 进行浏览。  
  
 下面的代码示例显示了当样式表中的某个 `variable` 或 `parameter` 元素计算结果为节点集时如何循环访问节点集。  
  
## <a name="style-sheet"></a>样式表  
  
```xml  
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" version="1.0">  
  
    <xsl:variable name="x" select="bookstore/book/title"></xsl:variable>  
  
    <xsl:template match="/">  
        <xsl:for-each select="$x">  
            ******  
            <xsl:value-of select="."></xsl:value-of>  
            ******  
        </xsl:for-each>  
    </xsl:template>  
  
</xsl:stylesheet>  
```  
  
## <a name="input"></a>输入  
  
```xml  
<bookstore>  
   <book style="autobiography">  
      <title>Seven Years in Trenton</title>  
   </book>  
  
   <book style="autobiography">  
      <title>Seven Years in Trenton2</title>  
   </book>  
  
   <book style="textbook">  
      <title>History of Trenton Vol 3</title>  
   </book>  
</bookstore>  
```  
  
## <a name="output"></a>Output  
  
```output  
******  
Seven Years in Trenton  
******  
  
******  
Seven Years in Trenton2  
******  
  
******  
History of Trenton Vol 3  
******  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Xml.XPath.XPathNodeIterator>
- [XslTransform 类的 XSLT 转换](xslt-transformations-with-the-xsltransform-class.md)
- [XslTransform 类实现 XSLT 处理器](xsltransform-class-implements-the-xslt-processor.md)
