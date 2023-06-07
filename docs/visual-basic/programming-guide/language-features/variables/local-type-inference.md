---
title: 局部类型推理
ms.date: 07/20/2015
f1_keywords:
- local type inference
- vb.TypeInfer
helpviewer_keywords:
- Option Infer statement [Visual Basic]
- local type inference [Visual Basic]
- implicitly-typed local variables [Visual Basic]
- variables [Visual Basic], type inference
- inference [Visual Basic]
- type inference [Visual Basic]
ms.assetid: b8307f18-2e56-4ab3-a45a-826873f400f6
ms.openlocfilehash: 3979396d32aa5d3b853aa087d43f70d5987e510b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410394"
---
# <a name="local-type-inference-visual-basic"></a>局部类型推理 (Visual Basic)

Visual Basic 编译器使用*类型推理*来确定在没有子句的情况下声明的局部变量的数据类型 `As` 。 编译器从初始化表达式的类型推断出变量的类型。 这样，便可以在不显式声明类型的情况下声明变量，如下面的示例中所示。 作为声明的结果， `num1` 和 `num2` 均强类型化为整数。

[!code-vb[VbVbalrTypeInference#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#1)]

> [!NOTE]
> 如果你不希望 `num2` 在前一示例中将类型化为 `Integer` ，则可以使用或等声明指定另一种类型 `Dim num3 As Object = 3` `Dim num4 As Double = 3` 。

> [!NOTE]
> 类型推理只能用于非静态局部变量;它不能用于确定类字段、属性或函数的类型。

局部类型推理适用于过程级别。 它不能用于在模块级别（在类、结构、模块或接口中，而不是在过程或块中）声明变量。 如果 `num2` 在前面的示例中是某个类的字段，而不是过程中的局部变量，则声明会导致 `Option Strict` 在上出现错误，并将 `num2` `Object` 使用 off 作为进行分类 `Option Strict` 。 同样，局部类型推理不适用于声明为的过程级别变量 `Static` 。

## <a name="type-inference-vs-late-binding"></a>类型推理与后期绑定

使用类型推理的代码与依赖后期绑定的代码类似。 但是，类型推理强类型变量，而不是将其保留原样 `Object` 。 编译器在编译时使用变量的初始值设定项来确定变量的类型以生成早期绑定的代码。 在上一个示例中，like 为， `num2` `num1` 类型为 `Integer` 。

早期绑定变量的行为与后期绑定变量的行为不同，后者的类型仅在运行时已知。 事先知道类型使编译器能够在执行之前识别问题，精确分配内存并执行其他优化。 早期绑定还允许 Visual Basic 集成开发环境（IDE）为对象的成员提供 IntelliSense 帮助。 早期绑定也是性能的首选。 这是因为，后期绑定变量中存储的所有数据都必须包装为类型 `Object` ，并且在运行时访问该类型的成员会使程序运行较慢。

## <a name="examples"></a>示例

如果在没有子句的情况下声明了局部变量并进行了初始化，则会发生类型推理 `As` 。 编译器使用所赋的初始值的类型作为变量的类型。 例如，下面的每一行代码声明一个类型为的变量 `String` 。

[!code-vb[VbVbalrTypeInference#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#2)]

下面的代码演示了两种创建整数数组的等效方法。

[!code-vb[VbVbalrTypeInference#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#3)]

使用类型推理来确定循环控制变量的类型是一种很方便的方法。 在下面的代码中，编译器将推断，这 `number` 是 `Integer` 因为 `someNumbers2` 前面的示例是一个整数数组。

[!code-vb[VbVbalrTypeInference#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#4)]

可以在语句中使用局部类型推理 `Using` 来建立资源名称的类型，如下例所示。

[!code-vb[VbVbalrTypeInference#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#7)]

还可以从函数的返回值推断变量的类型，如下例所示。 `pList1`和 `pList2` 都是进程的数组，因为 `Process.GetProcesses` 返回进程的数组。

[!code-vb[VbVbalrTypeInference#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTypeInference/VB/Class1.vb#5)]

## <a name="option-infer"></a>Option Infer

`Option Infer`使您能够指定是否允许在特定文件中进行局部类型推理。 若要启用或阻止该选项，请在文件开头键入以下语句之一。

`Option Infer On`

`Option Infer Off`

如果在代码中未指定的值 `Option Infer` ，则编译器默认值为 `Option Infer On` 。

如果为文件中 `Option Infer` 设置的值与在 IDE 中或在命令行上设置的值冲突，则文件中的值优先。

有关详细信息，请参阅[选项推断语句](../../../language-reference/statements/option-infer-statement.md)和[编译页，项目设计器（Visual Basic）](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)。

## <a name="see-also"></a>另请参阅

- [匿名类型](../objects-and-classes/anonymous-types.md)
- [早期绑定和后期绑定](../early-late-binding/index.md)
- [For Each...Next 语句](../../../language-reference/statements/for-each-next-statement.md)
- [For...Next 语句](../../../language-reference/statements/for-next-statement.md)
- [Option Infer 语句](../../../language-reference/statements/option-infer-statement.md)
- [-optioninfer](../../../reference/command-line-compiler/optioninfer.md)
- [Visual Basic 中的 LINQ 简介](../linq/introduction-to-linq.md)
