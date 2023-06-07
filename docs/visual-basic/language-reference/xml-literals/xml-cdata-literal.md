---
title: XML CDATA 文本
ms.date: 07/20/2015
f1_keywords:
- vb.XmlLiteralCdata
helpviewer_keywords:
- CDATA literal [Visual Basic]
- XML CDATA literal [Visual Basic]
- XML literals [Visual Basic], CDATA
ms.assetid: 9eafb6a4-dd9d-4866-85e8-0654c65abc44
ms.openlocfilehash: 4447ad6cf0fb251b0d2d1387c109b06d32f69cb8
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90866094"
---
# <a name="xml-cdata-literal-visual-basic"></a>XML CDATA 文本 (Visual Basic)

表示对象的文本 <xref:System.Xml.Linq.XCData> 。  
  
## <a name="syntax"></a>语法  
  
```xml  
<![CDATA[content]]>  
```  
  
## <a name="parts"></a>组成部分  

 `<![CDATA[`  
 必需。 表示 XML CDATA 部分的开头。  
  
 `content`  
 必需。 要显示在 XML CDATA 节中的文本内容。  
  
 `]]>`  
 必需。 表示部分的结尾。  
  
## <a name="return-value"></a>返回值  

 一个 <xref:System.Xml.Linq.XCData> 对象。  
  
## <a name="remarks"></a>备注  

 XML CDATA 节包含应包含的原始文本，但不应在包含它的 XML 中进行分析。 XML CDATA 节可以包含任何文本。 其中包括保留的 XML 字符。 XML CDATA 部分以序列 "]] 结尾 >"。 这意味着：  
  
- 不能在 XML CDATA 文本中使用嵌入式表达式，因为嵌入式表达式分隔符是有效的 XML CDATA 内容。  
  
- XML CDATA 节不能嵌套，因为 `content` 不能包含值 "]] >"。  
  
 可以将 XML CDATA 文本分配给一个变量，也可以将其包含在 XML 元素文本中。  
  
> [!NOTE]
> XML 文本可以跨多行，但不使用行继续符。 这使你可以从 XML 文档复制内容并将其直接粘贴到 Visual Basic 程序。  
  
 Visual Basic 编译器会将 XML CDATA 文本转换为对 <xref:System.Xml.Linq.XCData.%23ctor%2A> 构造函数的调用。  
  
## <a name="example"></a>示例  

 下面的示例创建一个 CDATA 节，其中包含文本 "可以包含文本 \<XML> 标记"。  
  
 [!code-vb[VbXMLSamples#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples11.vb#23)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Xml.Linq.XCData>
- [XML 元素文本](xml-element-literal.md)
- [XML 文本](index.md)
- [在 Visual Basic 中创建 XML](../../programming-guide/language-features/xml/creating-xml.md)
