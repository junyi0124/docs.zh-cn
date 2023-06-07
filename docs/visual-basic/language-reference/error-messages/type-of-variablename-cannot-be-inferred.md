---
title: 无法推断“<variablename>”的类型，因为循环边界和步骤变量未扩大到同一类型
ms.date: 07/20/2015
f1_keywords:
- bc30982
- vbc30982
helpviewer_keywords:
- BC30982
ms.assetid: 741e85d9-a747-42ad-a1e1-a3f1928aaff5
ms.openlocfilehash: 1330cbd6567b69df9bd811ced49c6df2e120a0b2
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161204"
---
# <a name="bc30982-type-of-variablename-cannot-be-inferred-because-the-loop-bounds-and-the-step-variable-do-not-widen-to-the-same-type"></a>BC30982：无法推断 "" 的类型， \<variablename> 因为循环边界和步骤变量未扩大到同一类型

你编写了一个 `For...Next` 循环，该循环中编译器无法推断循环控制变量的数据类型，因为满足以下条件：

- 未在 `As` 子句中指定循环控制变量的数据类型。

- 循环边界和步骤变量包含至少两种数据类型。

- 数据类型之间不存在标准转换。

 因此，编译器无法推断循环控制变量的数据类型。

 在下面的示例中，步骤变量是一个字符，并且循环边界都是整数。 由于字符和整数之间没有标准转换，因此将报告此错误。

```vb
Dim stepVar = "1"c
Dim m = 0
Dim n = 20

' Not valid.
' For i = 1 To 10 Step stepVar
    ' Loop processing
' Next
```

**错误 ID：** BC30982

## <a name="to-correct-this-error"></a>更正此错误

- 根据需要更改循环边界和步骤变量的类型，以使其中至少有一个类型为其他类型。 在前面的示例中，将的类型更改 `stepVar` 为 `Integer` 。

  ```vb
  Dim stepVar = 1
  ```

  或

  ```vb
  Dim stepVar As Integer = 1
  ```

- 使用显式转换函数将循环边界和步骤变量转换为适当的类型。 在前面的示例中，将 `Val` 函数应用于 `stepVar` 。

  ```vb
  For i = 1 To 10 Step Val(stepVar)
      ' Loop processing
  Next
  ```

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Conversion.Val%2A>
- [For...Next 语句](../statements/for-next-statement.md)
- [隐式转换和显式转换](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [局部类型推理](../../programming-guide/language-features/variables/local-type-inference.md)
- [Option Infer 语句](../statements/option-infer-statement.md)
- [Type Conversion Functions](../functions/type-conversion-functions.md)
- [Widening and Narrowing Conversions](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
