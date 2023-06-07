---
title: XSLT 转换
ms.date: 03/30/2017
ms.assetid: 202f8820-224c-494f-b61e-cd127eac6e03
ms.openlocfilehash: 686b2d454eda99095bcb335c8b0edba9ecf2e304
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95685053"
---
# <a name="xslt-transformations"></a>XSLT 转换

可扩展样式表语言转换 (XSLT) 可以将源 XML 文档的内容转换为另一个格式或结构不同的文档。 例如，可以使用 XSLT 将 XML 转换为在网站上使用的 HTML 或转换为只包含应用程序所需字段的文档。 此转换过程由 [W3C XSL 转换 (XSLT) 版本 1.0 建议](https://www.w3.org/TR/xslt-10/)规定。  
  
 <xref:System.Xml.Xsl.XslCompiledTransform> 类是 .NET 中的 XSLT 处理器。 <xref:System.Xml.Xsl.XslCompiledTransform> 类支持 [W3C XSLT 1.0 建议](https://www.w3.org/TR/xslt-10/)。  
  
> [!NOTE]
> <xref:System.Xml.Xsl.XslTransform> 类在 .NET Framework 2.0 版中已过时。 <xref:System.Xml.Xsl.XslCompiledTransform> 类是 XSLT 引擎的新实现。 它包括性能改进和新的安全功能。 建议的做法是使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类创建 XSLT 应用程序。  
  
## <a name="in-this-section"></a>本节内容  

 [使用 XslCompiledTransform 类](using-the-xslcompiledtransform-class.md)  
 提供如何使用 <xref:System.Xml.Xsl.XslCompiledTransform> 类的信息。  
  
 [从 XslTransform 类迁移](migrating-from-the-xsltransform-class.md)  
 讨论如何从 <xref:System.Xml.Xsl.XslTransform> 类迁移代码。  
  
 [XSLT 编译器 (xsltc.exe)](xslt-compiler-xsltc-exe.md)  
 提供有关如何使用 XSLT 编译器的信息。  
  
 [XslTransform 类的 XSLT 转换](xslt-transformations-with-the-xsltransform-class.md)  
 提供如何使用 <xref:System.Xml.Xsl.XslTransform> 类的信息。  
  
## <a name="reference"></a>参考  

 <xref:System.Xml.Xsl.XslCompiledTransform>  
 <xref:System.Xml.Xsl.XsltArgumentList>  
 <xref:System.Xml.Xsl.XsltSettings>  
  
## <a name="related-sections"></a>相关章节  

 [XML 文档和数据](index.md)
