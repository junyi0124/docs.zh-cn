---
title: 二元“If”表达式中的第一个操作数必须是可以为 null 的类型或引用类型
ms.date: 07/20/2015
f1_keywords:
- bc33107
- vbc33107
helpviewer_keywords:
- BC33107
ms.assetid: 493c8899-3f6b-4471-8eb6-9284e8492768
ms.openlocfilehash: bca9b74a68815b4e5a3bb2dc114b9031cdf24099
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162734"
---
# <a name="bc33107-first-operand-in-a-binary-if-expression-must-be-nullable-or-a-reference-type"></a>BC33107：二进制 "If" 表达式中的第一个操作数必须可以为 null 或引用类型

`If`表达式可以采用两个或三个参数。 当只发送两个参数时，第一个参数必须是引用类型或可以为 null 的值类型。 如果第一个参数的计算结果不是 `Nothing` ，则返回其值。 如果第一个参数的计算结果为 `Nothing` ，则计算并返回第二个参数。

 例如，下面的代码包含两个 `If` 表达式，一个具有三个参数，一个具有两个参数。 表达式计算并返回相同的值。

```vb
' firstChoice is a nullable value type.
Dim firstChoice? As Integer = Nothing
Dim secondChoice As Integer = 1128
' If expression with three arguments.
Console.WriteLine(If(firstChoice IsNot Nothing, firstChoice, secondChoice))
' If expression with two arguments.
Console.WriteLine(If(firstChoice, secondChoice))
```

 以下表达式将导致此错误：

```vb
Dim choice1 = 4
Dim choice2 = 5
Dim booleanVar = True

' Not valid.
'Console.WriteLine(If(choice1 < choice2, 1))
' Not valid.
'Console.WriteLine(If(booleanVar, "Test returns True."))
```

 **错误 ID：** BC33107

## <a name="to-correct-this-error"></a>更正此错误

- 如果无法更改代码，使第一个参数是可以为 null 的值类型或引用类型，请考虑将转换为三参数 `If` 表达式或 `If...Then...Else` 语句。

```vb
Console.WriteLine(If(choice1 < choice2, 1, 2))
Console.WriteLine(If(booleanVar, "Test returns True.", "Test returns False."))
```

## <a name="see-also"></a>另请参阅

- [If 运算符](../operators/if-operator.md)
- [If...Then...Else 语句](../statements/if-then-else-statement.md)
- [可以为 null 的值类型](../../programming-guide/language-features/data-types/nullable-value-types.md)
