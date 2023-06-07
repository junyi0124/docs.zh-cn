---
title: 如何：将十六进制字符串转换为数字
ms.date: 01/31/2018
helpviewer_keywords:
- numbers [Visual Basic], hexadecimals
- hexadecimals [Visual Basic], decimals
- examples [Visual Basic], string conversion
- decimals [Visual Basic], hexadecimals
- string conversion [Visual Basic], hexadecimal to numbers
ms.assetid: 76675807-eadb-4c08-bd50-e6c6ff4b8ced
ms.openlocfilehash: f0a97a0c212a64bfa4db4606ee526b666f07877a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347177"
---
# <a name="how-to-convert-hexadecimal-strings-to-numbers-visual-basic"></a>如何：将十六进制字符串转换为数字 (Visual Basic)

此示例使用 <xref:System.Convert.ToInt32%2A?displayProperty=nameWithType> 方法将十六进制字符串转换为整数。

## <a name="to-convert-a-hexadecimal-string-to-a-number"></a>将十六进制字符串转换为数字

- 使用 <xref:System.Convert.ToInt32(System.String,System.Int32)> 方法将以16为基数的数字转换为整数。

  <xref:System.Convert.ToInt32(System.String,System.Int32)> 方法的第一个参数是要转换的字符串。 第二个参数描述数字的表示形式;十六进制以16为基数。

  [!code-vb[VbVbalrStrings#62](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#62)]

- 请注意，十六进制字符串具有以下限制：

  - 它不能包含 `&h` 前缀。
  - 它不能包含 `_` 数字分隔符。

  如果前缀或数字分隔符存在，则对 <xref:System.Convert.ToInt32(System.String,System.Int32)> 方法的调用将引发 <xref:System.FormatException>。

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Conversion.Hex%2A>
- <xref:System.Convert.ToInt32%2A?displayProperty=nameWithType>
