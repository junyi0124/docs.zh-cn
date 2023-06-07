---
title: 如何：将字符串与模式相匹配
ms.date: 07/20/2015
helpviewer_keywords:
- comparison operators [Visual Basic], comparing strings
- pattern matching
- strings [Visual Basic], comparing
- string comparison [Visual Basic], operators
- Visual Basic code, operators
- pattern matching [Visual Basic], string comparison
- string comparison [Visual Basic]
- Like operator [Visual Basic], pattern matching
- pattern matching, empty strings
- operators [Visual Basic], comparison
ms.assetid: 19a83804-b5af-4739-928b-ac93e64e457f
ms.openlocfilehash: d8a3c363d1a443db4a0b7633e380562af1913aca
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403441"
---
# <a name="how-to-match-a-string-against-a-pattern-visual-basic"></a>如何：将字符串与模式相匹配 (Visual Basic)

如果要了解[字符串数据类型](../../../language-reference/data-types/string-data-type.md)的表达式是否满足模式，则可以使用[Like 运算符](../../../language-reference/operators/like-operator.md)。

`Like`采用两个操作数。 左操作数是一个字符串表达式，右操作数是包含要用于匹配的模式的字符串。 `Like`返回一个 `Boolean` 值，该值指示字符串表达式是否满足此模式。

可以将字符串表达式中的每个字符与特定字符、通配符、字符列表或字符范围匹配。 模式字符串中规范的位置与字符串表达式中要匹配的字符的位置相对应。

## <a name="to-match-a-character-in-the-string-expression-against-a-specific-character"></a>根据特定字符匹配字符串表达式中的字符

将特定字符直接放置在模式字符串中。 某些特殊字符必须括在方括号（ `[ ]` ）中。 有关详细信息，请参阅[Like 运算符](../../../language-reference/operators/like-operator.md)。

下面的示例测试是否 `myString` 完全包含单个字符 `H` 。

[!code-vb[VbVbalrOperators#70](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#70)]

## <a name="to-match-a-character-in-the-string-expression-against-a-wildcard-character"></a>将字符串表达式中的字符与通配符匹配

将问号（）放置 `?` 在模式字符串中。 此位置中的任何有效字符都使匹配成功。

下面的示例测试是否 `myString` 由单个字符 `W` 后跟任意值的两个字符组成。

[!code-vb[VbVbalrOperators#71](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#71)]

## <a name="to-match-a-character-in-the-string-expression-against-a-list-of-characters"></a>根据字符列表匹配字符串表达式中的字符

将括号（ `[ ]` ）放在模式字符串中，并将字符列表放在括号内。 不要将字符与逗号或任何其他分隔符分隔开。 列表中的任何单个字符均可成功匹配。

下面的示例将测试是否 `myString` 包含后跟一个字符、或的任何有效字符 `A` `C` `E` 。

[!code-vb[VbVbalrOperators#72](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#72)]

请注意，此匹配区分大小写。

## <a name="to-match-a-character-in-the-string-expression-against-a-range-of-characters"></a>将字符串表达式中的字符与一系列字符匹配

将括号（ `[ ]` ）放在模式字符串中，并将字符放入范围内的最小字符和最大字符，用连字符（ `–` ）分隔。 范围内的任何单个字符都使匹配成功。

下面的示例将测试是否 `myString` 包含 `num` 后跟一个字符 `i` 、、、、或的 `j` 字符 `k` `l` `m` `n` 。

[!code-vb[VbVbalrOperators#73](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#73)]

请注意，此匹配区分大小写。

## <a name="matching-empty-strings"></a>匹配的空字符串

`Like`将序列视为 `[]` 长度为零的字符串（ `""` ）。 您可以使用 `[]` 测试整个字符串表达式是否为空，但不能使用它来测试字符串表达式中的特定位置是否为空。 如果空位置是需要测试的选项之一，则可以使用多次 `Like` 。

### <a name="to-match-a-character-in-the-string-expression-against-a-list-of-characters-or-no-character"></a>将字符串表达式中的字符与字符或字符列表匹配

1. 在 `Like` 同一字符串表达式上两次调用运算符，并将两个调用连接到[或运算符](../../../language-reference/operators/or-operator.md)或[OrElse 运算符](../../../language-reference/operators/orelse-operator.md)。

2. 在第一个子句的模式字符串中 `Like` ，包含用方括号（）括起来的字符列表 `[ ]` 。

3. 在第二个子句的模式字符串中 `Like` ，不要将任何字符置于相关位置。

    下面的示例测试七位数的电话号码，该号码 `phoneNum` 只包含三个数字，后跟一个空格、一个连字符（ `–` ）、一个句点（ `.` ）或不包含任何字符，然后再后跟四个数字。

    [!code-vb[VbVbalrOperators#74](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#74)]

## <a name="see-also"></a>另请参阅

- [比较运算符](../../../language-reference/operators/comparison-operators.md)
- [运算符和表达式](index.md)
- [Like 运算符](../../../language-reference/operators/like-operator.md)
- [String 数据类型](../../../language-reference/data-types/string-data-type.md)
