---
title: 如何：从文件、字符串或流加载 XML
ms.date: 07/20/2015
helpviewer_keywords:
- XML [Visual Basic], loading
- LINQ to XML [Visual Basic], loading XML from files
ms.assetid: 2b02dcec-4cca-4575-b4ad-89ceb87b984c
ms.openlocfilehash: 7f2a961ebb7ecd4fc0512e141b4a437be87bec0e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84392283"
---
# <a name="how-to-load-xml-from-a-file-string-or-stream-visual-basic"></a>如何：从文件、字符串或流加载 XML (Visual Basic)

通过使用几种方法，可以创建[XML 文本](../../../language-reference/xml-literals/index.md)，并使用外部源（例如文件、字符串或流）中的内容填充这些文本。 下面的示例演示了这些方法。

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="to-load-xml-from-a-file"></a>从文件加载 XML

若要从文件填充 XML 文本（如 <xref:System.Xml.Linq.XElement> 或 <xref:System.Xml.Linq.XDocument> 对象），请使用 `Load` 方法。 此方法可采用文件路径、文本流或 XML 流作为输入。

下面的代码示例演示 <xref:System.Xml.Linq.XDocument.Load%28System.String%29> 如何使用方法 <xref:System.Xml.Linq.XDocument> 从文本文件中填充具有 XML 的对象。

[!code-vb[VbXMLSamples#43](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples15.vb#43)]

## <a name="to-load-xml-from-a-string"></a>从字符串加载 XML

若要从字符串填充 XML 文本（如 <xref:System.Xml.Linq.XElement> 或 <xref:System.Xml.Linq.XDocument> 对象），可以使用 `Parse` 方法。

下面的代码示例演示如何使用方法通过 <xref:System.Xml.Linq.XDocument.Parse%28System.String%29?displayProperty=nameWithType> <xref:System.Xml.Linq.XDocument> 字符串中的 XML 来填充对象。

[!code-vb[VbXMLSamples#47](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples15.vb#47)]

## <a name="to-load-xml-from-a-stream"></a>从流加载 XML

若要从流中填充 XML 文本（如 <xref:System.Xml.Linq.XElement> 或 <xref:System.Xml.Linq.XDocument> 对象），可以使用 `Load` 方法或 <xref:System.Xml.Linq.XNode.ReadFrom%2A?displayProperty=nameWithType> 方法。

下面的代码示例演示 <xref:System.Xml.Linq.XNode.ReadFrom%2A> 如何使用方法 <xref:System.Xml.Linq.XDocument> 从 xml 流中填充具有 xml 的对象。

[!code-vb[VbXMLSamples#46](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples15.vb#46)]

## <a name="see-also"></a>另请参阅

- <xref:System.Xml.Linq.XDocument.Load%2A?displayProperty=nameWithType>
- <xref:System.Xml.Linq.XElement.Load%2A?displayProperty=nameWithType>
- <xref:System.Xml.Linq.XElement.Parse%2A?displayProperty=nameWithType>
- <xref:System.Xml.Linq.XDocument.Parse%2A?displayProperty=nameWithType>
- <xref:System.Xml.Linq.XNode.ReadFrom%2A?displayProperty=nameWithType>
- [XML 文本](../../../language-reference/xml-literals/index.md)
- [XML](index.md)
- [在 Visual Basic 中操作 XML](manipulating-xml.md)
