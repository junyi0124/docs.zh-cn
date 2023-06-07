---
title: 类型“typename”的操作数“IsNot”只能与“Nothing”比较，因为“typename”是一个可以为 null 的类型
ms.date: 07/20/2015
f1_keywords:
- bc32128
- vbc32128
helpviewer_keywords:
- BC32128
ms.assetid: 1155b23a-ad75-4bab-b9da-73f35c767a36
ms.openlocfilehash: 084978c1e047eebd60149af63c0ec9a1135225be
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163332"
---
# <a name="bc32128-isnot-operand-of-type-typename-can-only-be-compared-to-nothing-because-typename-is-a-nullable-type"></a>BC32128：类型 "typename" 的 "IsNot" 操作数只能与 "Nothing" 比较，因为 "typename" 是一个可以为 null 的类型

已将声明为可以为 null 的值类型的变量与使用运算符之外的表达式进行比较 `Nothing` `IsNot` 。

**错误 ID：** BC32128

## <a name="to-correct-this-error"></a>更正此错误

要将可以为 null 的类型和表达式进行比较（而不是使用 `Nothing` 运算符比较 `IsNot` ），请调用可以为 null 的类型中的 `GetType` 方法并将结果与表达式进行比较，如下面的示例所示。

```vb
Dim number? As Integer = 5

If number IsNot Nothing Then
  If number.GetType() IsNot Type.GetType("System.Int32") Then

  End If
End If
```

## <a name="see-also"></a>另请参阅

- [可以为 null 的值类型](../../programming-guide/language-features/data-types/nullable-value-types.md)
- [IsNot 运算符](../operators/isnot-operator.md)
