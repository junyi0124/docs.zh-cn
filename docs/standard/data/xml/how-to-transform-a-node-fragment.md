---
title: 如何：转换节点片段
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 73a6c582-b9d7-4fa7-9a05-6d931e1f3de8
ms.openlocfilehash: f5eb8e7826dd132fd46f6f476335416e7dd03269
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722682"
---
# <a name="how-to-transform-a-node-fragment"></a>如何：转换节点片段

在转换 <xref:System.Xml.XmlDocument> 或 <xref:System.Xml.XPath.XPathDocument> 对象中包含的数据时，XSLT 转换应用于整个文档。 换句话说，如果你传入文档根节点以外的一个节点，并不能防止转换进程访问已加载文档的所有节点。 若要转换节点片段，必须创建一个仅包含节点片段的独立对象，并将该对象传递给 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法。  
  
## <a name="procedures"></a>过程  
  
#### <a name="to-transform-a-node-fragment"></a>转换节点片断  
  
1. 创建一个包含源文档的对象。  
  
2. 找到要转换的节点片断。  
  
3. 创建仅包含该节点片断的独立对象。  
  
4. 将该节点片断传递给 <xref:System.Xml.Xsl.XslCompiledTransform.Transform%2A> 方法。  
  
## <a name="example"></a>示例  

 以下示例转换节点片断并将结果输出到控制台。  
  
 [!code-csharp[XSLT_NodeFrag#1](../../../../samples/snippets/csharp/VS_Snippets_Data/XSLT_NodeFrag/CS/xslt_frag.cs#1)]
 [!code-vb[XSLT_NodeFrag#1](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XSLT_NodeFrag/VB/xslt_frag.vb#1)]  
  
### <a name="input"></a>输入  
  
##### <a name="booksxml"></a>books.xml  

 [!code-xml[XML_Core_Files#1](../../../../samples/snippets/xml/VS_Snippets_Data/XML_Core_Files/XML/books.xml#1)]  
  
##### <a name="singlexsl"></a>single.xsl  

 [!code-xml[XSLT_NodeFrag#2](../../../../samples/snippets/xml/VS_Snippets_Data/XSLT_NodeFrag/XML/single.xsl#2)]  
  
### <a name="output"></a>Output  

 书名为 The Confidence Man。  
  
## <a name="see-also"></a>请参阅

- [使用 XslCompiledTransform 类](using-the-xslcompiledtransform-class.md)
