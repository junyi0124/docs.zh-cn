---
title: Exit 语句
ms.date: 07/20/2015
f1_keywords:
- vb.Exit
helpviewer_keywords:
- execution [Visual Basic], ending
- files [Visual Basic], closing
- programs [Visual Basic], quitting
- code, exiting
- Exit statement [Visual Basic]
- program termination
- execution [Visual Basic], stopping
ms.assetid: 760bfb32-5c3f-4bdb-a432-9a6001c92db7
ms.openlocfilehash: 1bfe81428fd3c50663fd8978e05c6a945cd47df8
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345941"
---
# <a name="exit-statement-visual-basic"></a>Exit 语句 (Visual Basic)

退出过程或块并将控制立即传输到过程调用或块定义后面的语句。

## <a name="syntax"></a>语法

```vb
Exit { Do | For | Function | Property | Select | Sub | Try | While }
```

## <a name="statements"></a>语句

 `Exit Do`  
 立即退出出现 `Do` 循环。 继续执行 `Loop` 语句后面的语句。 `Exit Do` 只能在 `Do` 循环内使用。 在嵌套的 `Do` 循环中使用时，`Exit Do` 退出最内层的循环，并将控制转移到下一个更高的嵌套级别。

 `Exit For`  
 立即退出出现 `For` 循环。 继续执行 `Next` 语句后面的语句。 `Exit For` 只能在 `For`...`Next` 或 `For Each`...`Next` 循环内使用。 在嵌套的 `For` 循环中使用时，`Exit For` 退出最内层的循环，并将控制转移到下一个更高的嵌套级别。

 `Exit Function`  
 立即退出显示的 `Function` 过程。 继续执行调用 `Function` 过程的语句后面的语句。 `Exit Function` 只能在 `Function` 过程中使用。

 若要指定一个返回值，可以在 `Exit Function` 语句前面的行上将值分配给函数名称。 若要分配返回值并在一个语句中退出函数，可以改为使用[Return 语句](return-statement.md)。

 `Exit Property`  
 立即退出显示的 `Property` 过程。 执行将继续执行调用 `Property` 过程的语句，即，语句请求或设置属性的值。 `Exit Property` 只能在属性的 `Get` 或 `Set` 过程中使用。

 若要在 `Get` 过程中指定返回值，可以在 `Exit Property` 语句前面的行上将值分配给函数名称。 若要分配返回值并在一个语句中退出 `Get` 过程，则可以改用 `Return` 语句。

 在 `Set` 过程中，`Exit Property` 语句与 `Return` 语句等效。

 `Exit Select`  
 立即退出显示该 `Select Case` 的块。 继续执行 `End Select` 语句后面的语句。 `Exit Select` 只能在 `Select Case` 语句内使用。

 `Exit Sub`  
 立即退出显示的 `Sub` 过程。 继续执行调用 `Sub` 过程的语句后面的语句。 `Exit Sub` 只能在 `Sub` 过程中使用。

 在 `Sub` 过程中，`Exit Sub` 语句与 `Return` 语句等效。

 `Exit Try`  
 会立即退出显示的 `Try` 或 `Catch` 块。 如果有一个，则继续执行 `Finally` 块，否则将继续执行 `End Try` 语句后面的语句。 `Exit Try` 只能在 `Try` 或 `Catch` 块内使用，而不能在 `Finally` 块内使用。

 `Exit While`  
 立即退出出现 `While` 循环。 继续执行 `End While` 语句后面的语句。 `Exit While` 只能在 `While` 循环内使用。 在嵌套的 `While` 循环中使用时，`Exit While` 将控制转移到循环，该循环在 `Exit While` 发生的循环之上的一个嵌套级别。

## <a name="remarks"></a>备注

不要将 `Exit` 语句与 `End` 语句混淆。 `Exit` 不定义语句的末尾。

## <a name="example"></a>示例

在下面的示例中，循环条件在 `index` 变量大于100时停止循环。 但是，循环中的 `If` 语句导致 `Exit Do` 语句在索引变量大于10时停止循环。

[!code-vb[VbVbalrStatements#133](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class10.vb#133)]

## <a name="example"></a>示例

下面的示例将返回值分配给函数名称 `myFunction`，然后使用 `Exit Function` 从函数返回：

[!code-vb[VbVbalrStatements#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#23)]

## <a name="example"></a>示例

下面的示例使用[Return 语句](return-statement.md)来分配返回值并退出函数：

[!code-vb[VbVbalrStatements#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#24)]

## <a name="see-also"></a>另请参阅

- [Continue 语句](continue-statement.md)
- [Do...Loop 语句](do-loop-statement.md)
- [End 语句](end-statement.md)
- [For Each...Next 语句](for-each-next-statement.md)
- [For...Next 语句](for-next-statement.md)
- [Function 语句](function-statement.md)
- [Return 语句](return-statement.md)
- [Stop 语句](stop-statement.md)
- [Sub 语句](sub-statement.md)
- [Try...Catch...Finally 语句](try-catch-finally-statement.md)
