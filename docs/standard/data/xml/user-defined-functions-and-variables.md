---
title: 用户定义的函数和变量
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 4772f20e-1e7f-496e-93c2-1484473be555
ms.openlocfilehash: c1f519dd539a266d61945ad221bccceaa1361779
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95675615"
---
# <a name="user-defined-functions-and-variables"></a>用户定义的函数和变量

<xref:System.Xml.XPath.XPathNavigator> 类提供了一组方法，用于与 <xref:System.Xml.XPath.XPathDocument> 数据进行交互。 可以通过实现由 XPath 查询表达式使用的扩展函数和变量，对标准 XPath 函数进行补充。 <xref:System.Xml.XPath.XPathExpression.SetContext%2A> 方法可以接受从 <xref:System.Xml.Xsl.XsltContext> 派生的用户定义的上下文。 用户定义的函数由自定义上下文解析。  
  
 扩展函数和变量有助于防范 XML 注入式攻击。 在这些情况下，用户输入可分配给自定义变量，并由扩展函数进行处理，而不像通过处理指令串联的原始输入一样。 扩展函数和变量包含用户输入，因此它仅仅按照设计者的意图对 XML 数据进行操作。  
  
 若要使用扩展，您需要实现一个自定义 <xref:System.Xml.Xsl.XsltContext> 类，以及用于支持扩展函数和变量的接口 <xref:System.Xml.Xsl.IXsltContextFunction> 和 <xref:System.Xml.Xsl.IXsltContextVariable>。 <xref:System.Xml.XPath.XPathExpression> 通过其 <xref:System.Xml.Xsl.XsltArgumentList> 将用户输入添加到自定义 <xref:System.Xml.Xsl.XsltContext> 中。  
  
 <xref:System.Xml.XPath.XPathExpression> 表示一个已编译的查询，<xref:System.Xml.XPath.XPathNavigator> 使用该查询来查找和处理由该表达式标识的节点。  
  
 下面的示例演示从 <xref:System.Xml.Xsl.XsltContext> 派生的自定义上下文类的实现。 代码中的注释描述了类成员及其在自定义函数中的用法。 这段代码之后还提供了函数和变量实现以及使用这些实现的示例应用程序。  
  
 [!code-csharp[XPathExtensionFunctions#2](../../../../samples/snippets/csharp/VS_Snippets_Data/xpathextensionfunctions/cs/xpathextensionfunctions.cs#2)]
 [!code-vb[XPathExtensionFunctions#2](../../../../samples/snippets/visualbasic/VS_Snippets_Data/xpathextensionfunctions/vb/xpathextensionfunctions.vb#2)]  
  
 下面的代码实现了 <xref:System.Xml.Xsl.IXsltContextFunction>。 实现 <xref:System.Xml.Xsl.IXsltContextFunction> 的类将解析和执行用户定义的函数。 此示例使用此声明标识的函数：`private int CountChar(string title, char charTocount)`。  
  
 代码注释描述了类成员。  
  
 [!code-csharp[XPathExtensionFunctions#3](../../../../samples/snippets/csharp/VS_Snippets_Data/xpathextensionfunctions/cs/xpathextensionfunctions.cs#3)]
 [!code-vb[XPathExtensionFunctions#3](../../../../samples/snippets/visualbasic/VS_Snippets_Data/xpathextensionfunctions/vb/xpathextensionfunctions.vb#3)]  
  
 下面的代码实现了 <xref:System.Xml.Xsl.IXsltContextVariable>。 此类在运行时解析 XPath 查询表达式中对用户定义的变量的引用。 由自定义 <xref:System.Xml.Xsl.XsltContext.ResolveVariable%2A> 类的重写 <xref:System.Xml.Xsl.XsltContext> 方法创建并返回此类的实例。  
  
 代码注释描述了类成员。  
  
 [!code-csharp[XPathExtensionFunctions#4](../../../../samples/snippets/csharp/VS_Snippets_Data/xpathextensionfunctions/cs/xpathextensionfunctions.cs#4)]
 [!code-vb[XPathExtensionFunctions#4](../../../../samples/snippets/visualbasic/VS_Snippets_Data/xpathextensionfunctions/vb/xpathextensionfunctions.vb#4)]  
  
 在前面的类定义的作用域内，下面的代码使用自定义函数来计算 `Tasks.xml` 文档的元素中的字符数。 代码中的注释描述了用于编译自定义函数并对 `Tasks.xml` 文档运行自定义函数的代码。  
  
 [!code-csharp[XPathExtensionFunctions#1](../../../../samples/snippets/csharp/VS_Snippets_Data/xpathextensionfunctions/cs/xpathextensionfunctions.cs#1)]
 [!code-vb[XPathExtensionFunctions#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/xpathextensionfunctions/vb/xpathextensionfunctions.vb#1)]  
  
 此示例使用下面的 XML 数据。  
  
 [!code-xml[XPathExtensionFunctions#5](../../../../samples/snippets/xml/VS_Snippets_Data/xpathextensionfunctions/XML/tasks.xml#5)]
