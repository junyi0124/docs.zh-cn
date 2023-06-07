---
title: 扩展方法
ms.date: 07/20/2015
f1_keywords:
- vb.ExtensionMethods
helpviewer_keywords:
- extending data types [Visual Basic]
- extension methods [Visual Basic]
ms.assetid: b8020aae-374d-46a9-bcb7-8cc2390b93b6
ms.openlocfilehash: a88756fce9137f89db1b6b8b007d528e98381830
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74341176"
---
# <a name="extension-methods-visual-basic"></a>扩展方法 (Visual Basic)

通过扩展方法，开发人员可以向已定义的数据类型添加自定义功能，而无需创建新的派生类型。 通过扩展方法，可以编写一个方法，该方法可以像调用现有类型的实例方法一样进行调用。

## <a name="remarks"></a>备注

扩展方法只能是 `Sub` 过程或 `Function` 过程。 不能定义扩展属性、字段或事件。 所有扩展方法都必须使用 <xref:System.Runtime.CompilerServices?displayProperty=nameWithType> 命名空间中的扩展属性进行标记，且必须在[模块](../../../language-reference/statements/module-statement.md)中定义 `<Extension>`。 如果在模块之外定义扩展方法，则 Visual Basic 编译器将生成错误[BC36551](../../../misc/bc36551.md)"只能在模块中定义扩展方法"。

扩展方法定义中的第一个参数指定该方法扩展的数据类型。 当方法运行时，第一个参数绑定到调用方法的数据类型的实例。

`Extension` 属性只能应用于 Visual Basic [`Module`](../../../language-reference/statements/module-statement.md)、 [`Sub`](../../../language-reference/statements/sub-statement.md)或[`Function`](../../../language-reference/statements/function-statement.md)。 如果将它应用于 `Class` 或 `Structure`，则 Visual Basic 编译器将生成错误[BC36550](../../../language-reference/error-messages/extension-attribute-can-be-applied-only-to-module-sub-or-function-declarations.md)，"Extension" 特性只能应用于 "Module"、"Sub" 或 "Function" 声明。

## <a name="example"></a>示例

下面的示例定义 <xref:System.String> 数据类型的 `Print` 扩展。 方法使用 `Console.WriteLine` 来显示字符串。 `aString`的 `Print` 方法的参数确定方法扩展了 <xref:System.String> 类。

[!code-vb[VbVbalrExtensionMethods#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/StringExtensions.vb#1)]

请注意，扩展方法定义标记有 `<Extension()>`的扩展属性。 标记定义方法的模块是可选的，但必须标记每个扩展方法。 必须导入 <xref:System.Runtime.CompilerServices> 才能访问扩展属性。

扩展方法只能在模块中声明。 通常，在其中定义扩展方法的模块与在其中调用扩展方法的模块不同。 如果需要，将导入包含扩展方法的模块，使其进入范围。 在包含 `Print` 的模块位于范围内后，可以调用方法，就像它是一个不采用任何参数的普通实例方法，如 `ToUpper`：

[!code-vb[VbVbalrExtensionMethods#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Class1.vb#2)]

下一个示例 `PrintAndPunctuate`也是 <xref:System.String>的扩展，这一次使用两个参数定义。 `aString`的第一个参数确定扩展方法扩展 <xref:System.String>。 `punc`的第二个参数旨在作为一串标点符号，当调用方法时，该字符串作为参数传入。 方法显示后跟标点符号的字符串。

[!code-vb[VbVbalrExtensionMethods#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Class2.vb#3)]

方法是通过在 `punc`的字符串参数中发送来调用的： `example.PrintAndPunctuate(".")`

下面的示例显示定义和调用 `Print` 和 `PrintAndPunctuate`。 <xref:System.Runtime.CompilerServices> 导入到定义模块中，以便能够访问扩展属性。

```vb
Imports System.Runtime.CompilerServices

Module StringExtensions

    <Extension()>
    Public Sub Print(aString As String)
        Console.WriteLine(aString)
    End Sub

    <Extension()>
    Public Sub PrintAndPunctuate(aString As String, punc As String)
        Console.WriteLine(aString & punc)
    End Sub
End Module
```

接下来，扩展方法将进入范围并被调用：

```vb
Imports ConsoleApplication2.StringExtensions

Module Module1

    Sub Main()
        Dim example As String = "Example string"
        example.Print()

        example = "Hello"
        example.PrintAndPunctuate(".")
        example.PrintAndPunctuate("!!!!")
    End Sub
End Module
```

运行这些或类似扩展方法所需的全部都是在范围内。 如果包含扩展方法的模块位于范围内，则它将显示在 IntelliSense 中，并可作为普通的实例方法调用。

请注意，当调用方法时，不会在中为第一个参数发送参数。 先前的方法定义中的参数 `aString` 绑定到 `example`，后者调用它们的 `String` 的实例。 编译器将使用 `example` 作为发送到第一个参数的参数。

如果为设置为 `Nothing`的对象调用扩展方法，则将执行扩展方法。 这不适用于普通实例方法。 可以在扩展方法中显式检查 `Nothing`。

## <a name="types-that-can-be-extended"></a>可扩展的类型

可以在可在 Visual Basic 参数列表中表示的大多数类型上定义扩展方法，其中包括：

- 类（引用类型）
- 结构（值类型）
- 界面
- 委派
- ByRef 和 ByVal 参数
- 泛型方法参数
- 阵列

因为第一个参数指定扩展方法扩展的数据类型，所以它是必需的，并且不能是可选的。 出于此原因，`Optional` 参数和 `ParamArray` 参数不能是参数列表中的第一个参数。

在后期绑定中不考虑扩展方法。 在下面的示例中，语句 `anObject.PrintMe()` 引发 <xref:System.MissingMemberException> 异常，这与第二个 `PrintMe` 扩展方法定义被删除的情况相同。

[!code-vb[VbVbalrExtensionMethods#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Class6.vb#9)]

## <a name="best-practices"></a>最佳实践

扩展方法提供了一种方便且功能强大的方法来扩展现有类型。 但是，若要成功使用它们，需要考虑一些要点。 这些注意事项主要适用于类库的作者，但它们可能会影响任何使用扩展方法的应用程序。

最常见的情况是，您添加到不属于您的类型的扩展方法比添加到您控制的类型的扩展方法更易受到攻击。 您不拥有的类可能会干扰您的扩展方法。

- 如果存在一个签名与调用语句中的参数兼容的可访问实例成员，并且不需要将自变量转换为参数，则将优先使用该实例方法来处理任何扩展方法。 因此，如果在某个时间点向类添加了相应的实例方法，则您依赖的现有扩展成员可能会变得不可访问。

- 扩展方法的作者无法阻止其他程序员编写可能优先于原始扩展的冲突扩展方法。

- 您可以通过将扩展方法置于其自己的命名空间中来提高可靠性。 然后，你的库的使用者可以包括命名空间或排除命名空间，或在命名空间中进行选择，与库的其余部分分开。

- 扩展接口可能比扩展类更安全，特别是在你不拥有接口或类时。 接口的更改将影响实现它的每个类。 因此，作者在接口中添加或更改方法可能会降低。 但是，如果某个类实现了两个具有相同签名的扩展方法的接口，则这两个扩展方法都不可见。

- 扩展您可以的最特定类型。 在类型的层次结构中，如果你选择派生了许多其他类型的类型，则引入实例方法或其他可能会干扰你的方法的扩展方法可能有一些层。

## <a name="extension-methods-instance-methods-and-properties"></a>扩展方法、实例方法和属性

如果范围内实例方法的签名与调用语句的参数兼容，则将优先选择该实例方法作为任何扩展方法。 即使扩展方法更匹配，实例方法也具有优先权。 在下面的示例中，`ExampleClass` 包含一个名为 `ExampleMethod` 的实例方法，该方法具有一个类型为 `Integer`的参数。 扩展方法 `ExampleMethod` 扩展 `ExampleClass`，并且具有一个 `Long`类型的参数。

[!code-vb[VbVbalrExtensionMethods#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Class4.vb#4)]

在以下代码中对 `ExampleMethod` 的第一次调用将调用扩展方法，因为 `arg1` 是 `Long` 的，并且仅与扩展方法中的 `Long` 参数兼容。 对 `ExampleMethod` 的第二次调用具有 `Integer` 参数 `arg2`，并调用实例方法。

[!code-vb[VbVbalrExtensionMethods#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Class4.vb#5)]

现在反转两种方法中参数的数据类型：

[!code-vb[VbVbalrExtensionMethods#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Class5.vb#6)]

这次 `Main` 中的代码同时调用实例方法。 这是因为 `arg1` 和 `arg2` 都具有与 `Long`的扩大转换，并且在这两种情况下，实例方法优先于扩展方法。

[!code-vb[VbVbalrExtensionMethods#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Class5.vb#7)]

因此，扩展方法不能替换现有的实例方法。 但是，当扩展方法与实例方法同名但签名不冲突时，这两种方法都可以访问。 例如，如果类 `ExampleClass` 包含不采用任何参数的名为 `ExampleMethod` 的方法，则允许具有相同名称但具有不同签名的扩展方法，如下面的代码所示。

[!code-vb[VbVbalrExtensionMethods#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrExtensionMethods/VB/Module3.vb#8)]

此代码的输出如下所示：

```console
Extension method
Instance method
```

对于属性，这种情况更简单：如果扩展方法与它所扩展的类的属性具有相同的名称，则扩展方法不可见且无法访问。

## <a name="extension-method-precedence"></a>扩展方法的优先级

如果两个具有相同签名的扩展方法处于范围内并且可访问，则将调用优先级较高的扩展方法。 扩展方法的优先级基于用于使方法进入范围的机制。 以下列表显示优先层次结构（从高到低）。

1. 在当前模块内定义的扩展方法。

2. 在当前命名空间中的数据类型或其父命名空间中定义的扩展方法，其优先级高于父命名空间的子命名空间。

3. 在当前文件的任何类型导入中定义的扩展方法。

4. 在当前文件中的任何命名空间导入中定义的扩展方法。

5. 在任何项目级类型导入中定义的扩展方法。

6. 在任何项目级命名空间导入中定义的扩展方法。

如果优先顺序不能解析多义性，则可以使用完全限定名称来指定要调用的方法。 如果前面的示例中的 `Print` 方法是在名为 `StringExtensions`的模块中定义的，则完全限定名称 `StringExtensions.Print(example)` 而不是 `example.Print()`。

## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.CompilerServices>
- <xref:System.Runtime.CompilerServices.ExtensionAttribute>
- [扩展方法](../../../../csharp/programming-guide/classes-and-structs/extension-methods.md)
- [Module 语句](../../../language-reference/statements/module-statement.md)
- [过程参数和自变量](procedure-parameters-and-arguments.md)
- [可选参数](optional-parameters.md)
- [参数数组](parameter-arrays.md)
- [属性概述](../../concepts/attributes/index.md)
- [范围 Visual Basic](../declared-elements/scope.md)
