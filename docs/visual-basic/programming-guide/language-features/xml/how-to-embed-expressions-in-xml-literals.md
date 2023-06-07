---
title: 如何：在 XML 文本中嵌入表达式
ms.date: 07/20/2015
helpviewer_keywords:
- embedded expressions [Visual Basic]
- XML literals [Visual Basic], embedded expressions
ms.assetid: 75016fad-0141-42de-8564-5051be29487e
ms.openlocfilehash: 5ce1386e6a1ff8ffce296f5cea694499633eb011
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071204"
---
# <a name="how-to-embed-expressions-in-xml-literals-visual-basic"></a>如何：在 XML 文本中嵌入表达式 (Visual Basic)

可以结合使用 XML 文本和嵌入式表达式来创建 XML 文档、片段或元素，该元素包含在运行时创建的内容。 下面的示例演示如何在运行时使用嵌入式表达式填充元素内容、属性和元素名称。  
  
 嵌入式表达式的语法是 `<%=` `exp` `%>` ，这与 ASP.NET 使用的语法相同。 有关详细信息，请参阅 [XML 中的嵌入式表达式](embedded-expressions-in-xml.md)。  
  
 你还可以使用 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] api 来创建 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 对象。 有关详细信息，请参阅 <xref:System.Xml.Linq.XElement>。  
  
## <a name="procedures"></a>过程  
  
#### <a name="to-insert-text-as-element-content"></a>插入文本作为元素内容  
  
- 下面的示例演示如何在 `contactName` 开始和结束名称元素之间插入变量中包含的文本。  
  
     [!code-vb[VbXMLSamples#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples14.vb#39)]  
  
     该示例产生下面的输出：  
  
    ```xml  
    <contact>  
      <name>Patrick Hines</name>  
    </contact>  
    ```  
  
#### <a name="to-insert-text-as-an-attribute-value"></a>插入文本作为属性值  
  
- 下面的示例演示如何插入变量中包含的文本 `phoneType` 作为属性的值 `type` 。  
  
     [!code-vb[VbXMLSamples#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples14.vb#40)]  
  
     该示例产生下面的输出：  
  
    ```xml  
    <contact>  
      <phone type="home">206-555-0144</phone>  
    </contact>  
    ```  
  
#### <a name="to-insert-text-for-an-element-name"></a>插入元素名称的文本  
  
- 下面的示例演示如何插入变量中包含的文本 `elementName` 作为元素的名称。  
  
     使用此方法创建元素时，必须使用标记将其关闭 \</> 。  
  
     [!code-vb[VbXMLSamples#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples14.vb#41)]  
  
     该示例产生下面的输出：  
  
    ```xml  
    <contact>  
      <name>Patrick Hines</name>  
    </contact>  
    ```  
  
## <a name="see-also"></a>请参阅

- [如何：创建 XML 文本](how-to-create-xml-literals.md)
- [XML 中的嵌入式表达式](embedded-expressions-in-xml.md)
- [在 Visual Basic 中创建 XML](creating-xml.md)
- [XML](index.md)
