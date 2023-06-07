---
title: 未设置对象变量或 With 块变量
ms.date: 07/20/2015
f1_keywords:
- vbrID91
ms.assetid: 2f03e611-f0ed-465c-99a2-a816e034faa3
ms.openlocfilehash: 5eff7622ce2a35cf2846c5141cede98ea033d708
ms.sourcegitcommit: ff5a4eb5cffbcac9521bc44a907a118cd7e8638d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92159880"
---
# <a name="object-variable-or-with-block-variable-not-set"></a>未设置对象变量或 With 块变量

正在引用无效的对象变量。 出现此错误的原因可能有多种：

- 声明了变量，但未指定类型。 如果在未指定类型的情况下声明了变量，则默认为类型 `Object` 。

    例如，使用声明的变量 `Dim x` 的类型将为类型 `Object;` `Dim x As String` `String` 。

    > [!TIP]
    > `Option Strict`语句不允许使用导致类型的隐式 `Object` 类型。 如果省略该类型，则会发生编译时错误。 请参阅 [Option Strict 语句](../statements/option-strict-statement.md)。

- 你正在尝试引用已设置为的对象 `Nothing` 。

- 您正在尝试访问未正确声明的数组变量的元素。

    例如， `products() As String` 如果您尝试引用数组的元素，则声明为的数组将触发错误 `products(3) = "Widget"` 。 数组没有元素，被视为对象。

- 在初始化块之前，您正尝试访问代码块中的代码 `With...End With` 。   `With...End With`必须通过执行 `With` 语句入口点初始化块。

> [!NOTE]
> 在 Visual Basic 或 VBA 的早期版本中，通过将值分配给变量而不使用 `Set` 关键字 (`x = "name"` 而不是) ，也会触发此错误 `Set x = "name"` 。 `Set`关键字在 Visual Basic .net 中不再有效。

## <a name="to-correct-this-error"></a>更正此错误

1. `Option Strict` `On` 通过将以下代码添加到文件的开头，将设置为：

    ```vb
    Option Strict On
    ```

    运行该项目时，将在 **错误列表** 中为没有类型指定的任何变量显示编译器错误。

2. 如果你不想启用 `Option Strict` ，请在代码中搜索没有类型 (指定的任何变量， `Dim x` 而不是 `Dim x As String`) 并将目标类型添加到声明。

3. 请确保未引用已设置为的对象变量 `Nothing` 。  在代码中搜索关键字 `Nothing` ，然后修改代码，使对象在引用之前不会设置为 `Nothing` 。

4. 在访问数组变量之前，请确保对其进行标注。 您可以在第一次创建数组时分配维度 (`Dim x(5) As String` 而不是 `Dim x() As String`) ，或者使用 `ReDim` 关键字在第一次访问数组之前设置数组的尺寸。

5. 请确保 `With` 通过执行 `With` 语句入口点初始化块。

## <a name="see-also"></a>另请参阅

- [对象变量声明](../../programming-guide/language-features/variables/object-variable-declaration.md)
- [ReDim 语句](../statements/redim-statement.md)
- [With...End With 语句](../statements/with-end-with-statement.md)
