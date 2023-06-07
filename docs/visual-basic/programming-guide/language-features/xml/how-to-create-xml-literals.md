---
title: 如何：创建 XML 文本
ms.date: 07/20/2015
helpviewer_keywords:
- XML literals [Visual Basic], creating
ms.assetid: 573a6db5-b14d-4e42-b356-8cc7e2d77745
ms.openlocfilehash: c7ad8d684dde31550b6e1b74c098d152b227f6c1
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91058217"
---
# <a name="how-to-create-xml-literals-visual-basic"></a>如何：创建 XML 文本 (Visual Basic)

您可以使用 XML 文本直接在代码中创建 XML 文档、片段或元素。 本主题中的示例演示如何创建包含三个子元素的 XML 元素以及如何创建 XML 文档。  
  
 你还可以使用 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] api 来创建 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] 对象。 有关详细信息，请参阅 <xref:System.Xml.Linq.XElement>。  
  
### <a name="to-create-an-xml-element"></a>创建 XML 元素  
  
- 使用 XML 文本语法（与实际 XML 语法相同）创建 XML 内联。  
  
     [!code-vb[VbXMLSamples#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples2.vb#5)]  
  
     运行代码。 此代码的输出为：  
  
     `<contact>`  
  
     `<name>Patrick Hines</name>`  
  
     `<phone type="home">206-555-0144</phone>`  
  
     `<phone type="work">425-555-0145</phone>`  
  
     `</contact>`  
  
### <a name="to-create-an-xml-document"></a>创建 XML 文档  
  
- 以内联方式创建 XML 文档。 下面的代码创建一个 XML 文档，该文档具有文本语法、XML 声明、处理指令、注释以及包含其他元素的元素。  
  
     [!code-vb[VbXMLSamples#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#30)]  
  
     运行代码。 此代码的输出为：  
  
     `<?xml-stylesheet type="text/xsl" href="show_book.xsl"?>`  
  
     `<!-- Tests that the application works. -->`  
  
     `<books>`  
  
     `<book/>`  
  
     `</books>`  
  
## <a name="see-also"></a>请参阅

- [XML](index.md)
- [在 Visual Basic 中创建 XML](creating-xml.md)
- [XML 元素文本](../../../language-reference/xml-literals/xml-element-literal.md)
- [XML 文档文本](../../../language-reference/xml-literals/xml-document-literal.md)
