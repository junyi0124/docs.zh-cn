---
title: 如何：在字符串内搜索
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], finding
- strings [Visual Basic], searching
- examples [Visual Basic], strings
ms.assetid: ae4c79e0-08ea-489f-bdb2-5eb6d355f284
ms.openlocfilehash: 655f746e4e496e1935afcd2a9f9fe36d9e3a2564
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348424"
---
# <a name="how-to-search-within-a-string-visual-basic"></a>如何：在字符串中搜索（Visual Basic）

本文举例说明如何在 Visual Basic 中的字符串内进行搜索。

## <a name="example"></a>示例

此示例对 <xref:System.String> 对象调用 <xref:System.String.IndexOf%2A> 方法，以报告子字符串的第一个匹配项的索引：

 [!code-vb[VbVbalrStrings#71](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#71)]

## <a name="robust-programming"></a>可靠编程

<xref:System.String.IndexOf%2A> 方法返回子字符串的第一个匹配项的第一个字符的位置。 索引从0开始，这意味着字符串的第一个字符的索引为0。

如果 <xref:System.String.IndexOf%2A> 未找到子字符串，则返回-1。

<xref:System.String.IndexOf%2A> 方法区分大小写，并使用当前区域性。

为了获得最佳的错误控制，你可能需要将字符串搜索包含 `Try` 在[Try .。。Catch .。。Finally 语句](../../../language-reference/statements/try-catch-finally-statement.md)构造。

## <a name="see-also"></a>另请参阅

- <xref:System.String.IndexOf%2A>
- [Try...Catch...Finally 语句](../../../language-reference/statements/try-catch-finally-statement.md)
- [Visual Basic 中的字符串简介](introduction-to-strings.md)
- [字符串](index.md)
