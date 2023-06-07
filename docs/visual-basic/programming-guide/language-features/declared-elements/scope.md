---
title: 范围
ms.date: 07/20/2015
helpviewer_keywords:
- module scope [Visual Basic]
- scope [Visual Basic], levels
- module level
- procedures [Visual Basic], scope
- declared elements [Visual Basic], scope
- namespaces [Visual Basic], scope
- scope [Visual Basic], declared elements
- scope [Visual Basic], about scope
- levels of scope [Visual Basic]
- block scope [Visual Basic]
- scope [Visual Basic], Visual Basic
- procedure scope [Visual Basic]
ms.assetid: 208106fe-79c9-4eec-93c6-55f08548895f
ms.openlocfilehash: 1bee904996257474b7457b2aefb1f17d250933cb
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410729"
---
# <a name="scope-in-visual-basic"></a>Visual Basic 中的范围

已声明元素的*作用域*是所有可引用它的代码的集合，无需限定其名称或通过[Imports 语句（.net 命名空间和类型）](../../../language-reference/statements/imports-statement-net-namespace-and-type.md)提供它。 元素可以具有以下级别之一的作用域：

|级别|说明|
|-----------|-----------------|
|块范围|仅在声明它的代码块内可用|
|过程范围|对声明它的过程中的所有代码可用|
|模块范围|适用于声明它的模块、类或结构中的所有代码|
|命名空间范围|可用于声明它的命名空间中的所有代码|

这种级别的作用域从最小（块）到最宽（命名空间）的范围进度，其中最*窄的范围*指的是可以引用元素而不进行限定的最小代码集。 有关详细信息，请参阅本页上的 "范围级别"。

## <a name="specifying-scope-and-defining-variables"></a>指定作用域和定义变量

在声明元素时指定其作用域。 范围可能取决于以下因素：

- 声明元素的区域（块、过程、模块、类或结构）

- 包含元素声明的命名空间

- 为元素声明的访问级别

定义名称相同但范围不同的变量时请小心，因为这样做可能会导致意外的结果。 有关详细信息，请参阅 [References to Declared Elements](references-to-declared-elements.md)。

## <a name="levels-of-scope"></a>范围级别

编程元素可在声明它的整个区域内使用。 同一区域中的所有代码都可以引用元素，而无需限定其名称。

### <a name="block-scope"></a>块范围

块是包含在启动和终止声明语句中的一组语句，如下所示：

- `Do` 和 `Loop`

- `For`[ `Each` ] 和`Next`

- `If` 和 `End If`

- `Select` 和 `End Select`

- `SyncLock` 和 `End SyncLock`

- `Try` 和 `End Try`

- `While` 和 `End While`

- `With` 和 `End With`

如果在块中声明一个变量，则只能在该块内使用它。 在下面的示例中，整数变量的作用域 `cube` 是介于和之间的块 `If` `End If` ， `cube` 当执行传递到块时，你将无法再引用。

```vb
If n < 1291 Then
    Dim cube As Integer
    cube = n ^ 3
End If
```

> [!NOTE]
> 即使变量的作用域限制为块，其生存期仍是整个过程的生存期。 如果在过程中多次输入块，则每个块变量将保留其以前的值。 若要避免在这种情况下出现意外结果，最好在块的开头初始化块变量。

### <a name="procedure-scope"></a>过程范围

在过程中声明的元素在该过程之外不可用。 只有包含声明的过程可以使用它。 此级别的变量也称为*局部变量*。 用[Dim 语句](../../../language-reference/statements/dim-statement.md)声明它们，无论是还是不带[Static](../../../language-reference/modifiers/static.md)关键字。

过程和块范围密切相关。 如果在过程内但在该过程中的任何块外声明变量，则可以将变量视为具有块范围，其中块是整个过程。

> [!NOTE]
> 所有本地元素（即使它们是 `Static` 变量）都专用于它们出现的过程。 不能在过程中使用[Public](../../../language-reference/modifiers/public.md)关键字声明任何元素。

### <a name="module-scope"></a>模块范围

为方便起见，单术语*模块级别*同样适用于模块、类和结构。 您可以通过将声明语句置于任何过程或块的外部，但在模块、类或结构中，在此级别声明元素。

在模块级别进行声明时，所选的访问级别将确定范围。 包含模块、类或结构的命名空间还会影响范围。

声明[专用](../../../language-reference/modifiers/private.md)访问级别的元素可用于该模块中的每个过程，但不能用于不同模块中的任何代码。 `Dim` `Private` 如果不使用任何访问级别关键字，模块级别的语句将默认为。 但是，可以在语句中使用关键字，使作用域和访问级别更明显 `Private` `Dim` 。

在下面的示例中，模块中定义的所有过程都可以引用字符串变量 `strMsg` 。 调用第二个过程时，将在对话框中显示字符串变量的内容 `strMsg` 。

```vb
' Put the following declaration at module level (not in any procedure).
Private strMsg As String
' Put the following Sub procedure in the same module.
Sub initializePrivateVariable()
    strMsg = "This variable cannot be used outside this module."
End Sub
' Put the following Sub procedure in the same module.
Sub usePrivateVariable()
    MsgBox(strMsg)
End Sub
```

### <a name="namespace-scope"></a>命名空间范围

如果使用[Friend](../../../language-reference/modifiers/friend.md)或[Public](../../../language-reference/modifiers/public.md)关键字在模块级别声明一个元素，则该元素将可用于声明该元素的整个命名空间中的所有过程。 对于前面的示例的以下更改，字符串变量 `strMsg` 可以在其声明的命名空间中的任何位置通过代码引用。

```vb
' Include this declaration at module level (not inside any procedure).
Public strMsg As String
```

命名空间范围包括嵌套命名空间。 命名空间中的可用元素也可以从嵌套在该命名空间中的任何命名空间中使用。

如果你的项目不包含任何[命名空间语句](../../../language-reference/statements/namespace-statement.md)，则项目中的所有内容都在相同的命名空间中。 在这种情况下，可以将命名空间范围视为项目范围。 `Public`模块、类或结构中的元素也可用于引用其项目的任何项目。

## <a name="choice-of-scope"></a>范围选择

当你声明变量时，在选择其作用域时应牢记以下几点。

### <a name="advantages-of-local-variables"></a>局部变量的优点

局部变量对于任何类型的临时计算都是一个不错的选择，原因如下：

- **避免名称冲突。** 局部变量名称不容易发生冲突。 例如，可以创建几个不同的过程，其中包含一个名为的变量 `intTemp` 。 只要每个 `intTemp` 都声明为局部变量，每个过程就只能识别自己的版本 `intTemp` 。 任何一个过程都可以更改其本地中的值， `intTemp` 而不会影响 `intTemp` 其他过程中的变量。

- **内存消耗。** 局部变量仅在其过程正在运行时占用内存。 当过程返回到调用代码时，将释放其内存。 相反，[共享](../../../language-reference/modifiers/shared.md)和[静态](../../../language-reference/modifiers/static.md)变量将消耗内存资源，直到应用程序停止运行，因此仅在必要时才使用它们。 *实例变量*会在其实例继续存在时使用内存，这使得它们的效率低于局部变量，但可能更高效 `Shared` `Static` 。

### <a name="minimizing-scope"></a>最小化范围

通常情况下，在声明任何变量或常量时，最好将范围设置为尽可能缩小（块范围是最小）。 这有助于节省内存，并最大程度地减少代码错误引用错误变量的几率。 同样，仅当需要在过程调用之间保留变量值时，才应将该变量声明为[静态](../../../language-reference/modifiers/static.md)。

## <a name="see-also"></a>另请参阅

- [已声明元素的特性](declared-element-characteristics.md)
- [如何：控制变量的范围](how-to-control-the-scope-of-a-variable.md)
- [Visual Basic 中的生存期](lifetime.md)
- [Visual Basic 中的访问级别](access-levels.md)
- [References to Declared Elements](references-to-declared-elements.md)
- [变量声明](../variables/variable-declaration.md)
