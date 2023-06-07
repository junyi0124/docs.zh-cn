---
title: XML 文档文本
ms.date: 07/20/2015
f1_keywords:
- vb.XmlLiteralDocument
helpviewer_keywords:
- XML document literal [Visual Basic]
- XML literals [Visual Basic], document
- XML documents [Visual Basic], creating
- document literal [Visual Basic]
ms.assetid: f7bbee56-0911-41de-b907-96f20450137b
ms.openlocfilehash: bd1b2f43fce563af431d67b3817b05c7c1048314
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866021"
---
# <a name="xml-document-literal-visual-basic"></a>XML 文档文本 (Visual Basic)

表示对象的文本 <xref:System.Xml.Linq.XDocument> 。  
  
## <a name="syntax"></a>语法  
  
```xml  
<?xml version="1.0" [encoding="encoding"] [standalone="standalone"] ?>  
[ piCommentList ]  
rootElement  
[ piCommentList ]  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`encoding`|可选。 声明文档使用的编码的文本文本。|  
|`standalone`|可选。 文本。 必须是 "是" 或 "否"。|  
|`piCommentList`|可选。 XML 处理指令和 XML 注释的列表。 采用以下格式：<br /><br /> `piComment [` `piComment` `... ]`<br /><br /> 每个 `piComment` 可以是以下各项之一：<br /><br /> -   [XML 处理指令文本](xml-processing-instruction-literal.md)。<br />-   [XML 注释文本](xml-comment-literal.md)。|  
|`rootElement`|必需。 文档的根元素。 格式为以下格式之一：<br /><br /> <ul><li>[XML 元素文本](xml-element-literal.md)。</li><li>窗体的嵌入式表达式 `<%=` `elementExp` `%>` 。 `elementExp`返回以下内容之一：<br /><br /> <ul><li>一个 <xref:System.Xml.Linq.XElement> 对象。</li><li>一个集合，其中包含一个 <xref:System.Xml.Linq.XElement> 对象和任意数量的 <xref:System.Xml.Linq.XProcessingInstruction> 和 <xref:System.Xml.Linq.XComment> 对象。</li></ul></li></ul><br /> 有关详细信息，请参阅 [XML 中的嵌入式表达式](../../programming-guide/language-features/xml/embedded-expressions-in-xml.md)。|  
  
## <a name="return-value"></a>返回值  

 一个 <xref:System.Xml.Linq.XDocument> 对象。  
  
## <a name="remarks"></a>备注  

 XML 文档文本由文本开头的 XML 声明标识。 虽然每个 XML 文档文本必须只有一个根 XML 元素，但是它可以有任意数量的 XML 处理指令和 XML 注释。  
  
 Xml 文档文字不能出现在 XML 元素中。  
  
> [!NOTE]
> XML 文本可以跨多行，而无需使用行继续符。 这使你可以从 XML 文档复制内容并将其直接粘贴到 Visual Basic 程序。  
  
 Visual Basic 编译器将 XML 文档文本转换为对 <xref:System.Xml.Linq.XDocument.%23ctor%2A> 和 <xref:System.Xml.Linq.XDeclaration.%23ctor%2A> 构造函数的调用。  
  
## <a name="example"></a>示例  

 下面的示例创建一个 XML 文档，该文档具有 XML 声明、处理指令、注释以及包含其他元素的元素。  
  
 [!code-vb[VbXMLSamples#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples13.vb#30)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Xml.Linq.XElement>
- <xref:System.Xml.Linq.XProcessingInstruction>
- <xref:System.Xml.Linq.XComment>
- <xref:System.Xml.Linq.XDocument>
- [XML 处理指令文本](xml-processing-instruction-literal.md)
- [XML 注释文本](xml-comment-literal.md)
- [XML 元素文本](xml-element-literal.md)
- [XML 文本](index.md)
- [在 Visual Basic 中创建 XML](../../programming-guide/language-features/xml/creating-xml.md)
- [XML 中的嵌入式表达式](../../programming-guide/language-features/xml/embedded-expressions-in-xml.md)
