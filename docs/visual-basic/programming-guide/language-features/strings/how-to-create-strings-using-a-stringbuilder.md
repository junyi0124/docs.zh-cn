---
title: 如何：使用字符串生成器创建字符串
ms.date: 07/20/2015
helpviewer_keywords:
- StringBuilder class
- strings [Visual Basic], using StringBuilder
ms.assetid: 9c042880-aa16-432e-9ccb-cd00abda9ae3
ms.openlocfilehash: c41db584df83782dab99b90043045aa2cabcb6ff
ms.sourcegitcommit: 62285ec11fa8e8424bab00511a90760c60e63c95
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/20/2020
ms.locfileid: "81645329"
---
# <a name="how-to-create-strings-using-a-stringbuilder-in-visual-basic"></a>如何：在可视化基础中使用字符串生成器创建字符串

此示例使用<xref:System.Text.StringBuilder>类从许多较小的字符串构造长字符串。 类<xref:System.Text.StringBuilder>比`&=`运算符串联许多字符串更有效。

## <a name="example"></a>示例

下面的示例创建类的<xref:System.Text.StringBuilder>实例，将 1，000 个字符串追加到该实例，然后返回其字符串表示形式：

 [!code-vb[VbVbalrStrings#70](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#70)]

## <a name="see-also"></a>另请参阅

- [使用 StringBuilder 类](../../../../standard/base-types/stringbuilder.md)
- [&= 运算符](../../../language-reference/operators/and-assignment-operator.md)
- [字符串](index.md)
- [新建字符串](../../../../standard/base-types/creating-new.md)
- [控制字符串](../../../../standard/base-types/best-practices-strings.md)
