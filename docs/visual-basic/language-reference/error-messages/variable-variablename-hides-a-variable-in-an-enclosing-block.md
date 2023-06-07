---
title: 变量“<variablename>”在封闭块中隐藏变量
ms.date: 07/20/2015
f1_keywords:
- vbc30616
- bc30616
helpviewer_keywords:
- BC30616
ms.assetid: e7658ebc-da45-451b-a409-a0f8915f0beb
ms.openlocfilehash: 408acaafd5e266266b5191313611b94b72a5c270
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92161343"
---
# <a name="bc30616-variable-variablename-hides-a-variable-in-an-enclosing-block"></a>BC30616：变量 " \<variablename> " 在封闭块中隐藏变量

块中包含的变量与另一个本地变量的名称相同。

 **错误 ID：** BC30616

## <a name="to-correct-this-error"></a>更正此错误

- 重命名封闭块中的变量，使其与其他任何局部变量不同。 例如：

    ```vb
    Dim a, b, x As Integer
    If a = b Then
       Dim y As Integer = 20 ' Uniquely named block variable.
    End If
    ```

- 此错误的一个常见原因是在 `Catch e As Exception` 事件处理程序内部使用。 如果是这种情况，请将 `Catch` 块变量命名为 `ex` 而不是 `e` 。

- 此错误的另一个常见原因是尝试访问在单独块内的块内声明的局部变量 `Try` `Catch` 。 若要更正此错误，请在结构外声明变量 `Try...Catch...Finally` 。

## <a name="see-also"></a>另请参阅

- [Try...Catch...Finally 语句](../statements/try-catch-finally-statement.md)
- [变量声明](../../programming-guide/language-features/variables/variable-declaration.md)
