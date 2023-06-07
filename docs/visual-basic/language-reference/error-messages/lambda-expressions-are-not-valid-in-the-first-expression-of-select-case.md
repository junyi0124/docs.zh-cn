---
title: Lambda 表达式在“Select Case”语句的第一个表达式中无效
ms.date: 07/20/2015
f1_keywords:
- bc36635
- vbc36635
helpviewer_keywords:
- BC36635
ms.assetid: 74609979-9c03-4864-bbce-f588aa2e0917
ms.openlocfilehash: 448a685a7c174ce212389842c31066f1c4557cdf
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92162526"
---
# <a name="bc36635-lambda-expressions-are-not-valid-in-the-first-expression-of-a-select-case-statement"></a>BC36635： Lambda 表达式在 "Select Case" 语句的第一个表达式中无效

不能对语句中的测试表达式使用 lambda 表达式 `Select Case` 。 Lambda 表达式定义返回函数，语句的测试表达式 `Select Case` 必须是基本数据类型。

 下面的代码会导致此错误：

```vb
' Select Case (Function(arg) arg Is Nothing)
    ' List of the cases.
' End Select
```

 **错误 ID：** BC36635

## <a name="to-correct-this-error"></a>更正此错误

- 检查你的代码以确定是否可以使用其他条件构造，例如 `If...Then...Else` 语句。

- 你可能打算调用函数，如以下代码所示：

```vb
Dim num? As Integer
Select Case ((Function(arg? As Integer) arg Is Nothing)(num))
    ' List of the cases
End Select
```

## <a name="see-also"></a>请参阅

- [Lambda 表达式](../../programming-guide/language-features/procedures/lambda-expressions.md)
- [If...Then...Else 语句](../statements/if-then-else-statement.md)
- [Select...Case 语句](../statements/select-case-statement.md)
