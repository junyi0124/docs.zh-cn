---
title: 函数“<procedurename>”并非在所有代码路径上都返回值
ms.date: 07/20/2015
f1_keywords:
- bc42105
- vbc42105
helpviewer_keywords:
- BC42105
ms.assetid: b6929bf4-a365-4a70-8dc9-6b0fc09e1468
ms.openlocfilehash: 19b305e337767dfb34718aed7b665f142851bd36
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92163358"
---
# <a name="bc42105-function-procedurename-doesnt-return-a-value-on-all-code-paths"></a>BC42105：函数 " \<procedurename> " 不会在所有代码路径上都返回值

函数 " \<procedurename> " 不会在所有代码路径上都返回值。 是否缺少 "Return" 语句？

 `Function`过程至少有一个可能的路径，其代码中不返回值。

 可以通过 `Function` 以下任一方式从过程返回值：

- 在 [Return 语句](../statements/return-statement.md)中包含值。

- 将值分配给 `Function` 过程名称，然后执行 `Exit Function` 语句。

- 将值分配给 `Function` 过程名称，然后执行 `End Function` 语句。

 如果控件传递给 `Exit Function` 或 `End Function` 并且您没有为过程名称赋值，则该过程将返回返回数据类型的默认值。 有关详细信息，请参阅 [Function 语句](../statements/function-statement.md)中的 "行为"。

 默认情况下，此消息是一个警告。 有关隐藏警告或将警告视为错误的详细信息，请参见 [Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)。

 **错误 ID：** BC42105

## <a name="to-correct-this-error"></a>更正此错误

- 检查控制流逻辑，并确保在导致返回的每个语句之前赋值。

     如果始终使用语句，则更容易保证每次从过程返回一个值 `Return` 。 如果执行此操作，则之前的最后一个语句 `End Function` 应为 `Return` 语句。

## <a name="see-also"></a>另请参阅

- [Function 过程](../../programming-guide/language-features/procedures/function-procedures.md)
- [Function 语句](../statements/function-statement.md)
- [“项目设计器”->“编译”页 (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)
